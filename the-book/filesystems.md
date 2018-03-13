# Filesystem Architecture

This document seeks to describe a high-level view of the Fuchsia filesystems,
from their initialization, discussion of standard filesystem operations (such as
Open, Read, Write, etc), and the quirks of implementing user-space filesystems
on top of a microkernel. Additionally, this document describes the VFS-level
walking through a namespace which can be used to communicate with non-storage
entities (such as system services).

## Filesystems are Services

Unlike more common monolithic kernels, Fuchsia’s filesystems live entirely
within userspace. They are not linked nor loaded with the kernel; they are
simply userspace processes which implement servers that can appear as
filesystems. As a consequence, Fuchsia’s filesystems themselves can be changed
with ease -- modifications don’t require recompiling the kernel. In fact,
updating to a new Fuchsia filesystem can be done without rebooting.

Like other native servers on Fuchsia, the primary mode of interaction with a
filesystem server is achieved using the handle primitive rather than system
calls. The kernel has no knowledge about files, directories, or filesystems. As
a consequence, filesystem clients cannot ask the kernel for “filesystem access”
directly.

This architecture implies that the interaction with filesystems is limited to
the following interface:

 * The messages sent on communication channels established with the filesystem
   server. These communication channels may be local for a client-side
   filesystem, or remote.
 * The initialization routine (which is expected to be configured heavily on a
   per-filesystem basis; a networking filesystem would require network access,
   persistent filesystems may require block device access, in-memory filesystems
   would only require a mechanism to allocate new temporary pages).

As a benefit of this interface, any resources accessible via a channel can make
themselves appear like filesystems by implementing the expected protocols for
files or directories. For example, “serviceFS” (discussed in more detail later
in this document) allows for service discovery through a filesystem interface.

## File Lifecycle

### Establishing a Connection

To open a file, Fuchsia programs (clients) send RPC requests to filesystem
servers using a protocol called [RemoteIO](life_of_an_open.md#RemoteIO).
RemoteIO defines a wire-format for transmitting messages and handles between a
filesystem client and server. Instead of interacting with a kernel-implemented
VFS layer, Fuchsia processes send requests to filesystem services which
implement protocols for Files, Directories, and Devices. To send one of these
open requests, a Fuchsia process must transmit an RPC message over an existing
handle to a directory; for more detail on this process, refer to the [life of an
open document](life_of_an_open.md).

### Namespaces

On Fuchsia, a [namespace](namespaces.md) is a small filesystem which exists
entirely within the client. At the most basic level, the idea of the client
saving “/” as root and associating a handle with it is a very primitive
namespace. Instead of a typical singular "global" filesystem namespace, Fuchsia
processes can be provided an arbitrary directory handle to represent "root",
limiting the scope of their namespace. In order to limit this scope, Fuchsia
filesystems [intentionally do not allow access to parent directories via
dotdot](dotdot.md).

Fuchsia processes may additionally redirect certain path operations to separate
filesystem servers. When a client refers to “/bin”, the client may opt to
redirect these requests to a local handle representing the “/bin” directory,
rather than sending a request directly to the “bin” directory within the “root”
directory. Namespaces, like all filesystem constructs, are not visible from the
kernel: rather, they are implemented in client-side runtimes (such as
[libfdio](life_of_an_open.md#Fdio)) and are interposed between most client code
and the handles to remote filesystems.

Since namespaces operate on handles, and most Fuchsia resources and services
are accessible through handles, they are extremely powerful concepts.
Filesystem objects (such as directories and files), services, devices,
packages, and environments (visible by privileged processes) all are usable
through handles, and may be composed arbitrarily within a child process. As a
result, namespaces allows for customizable resource discovery within
applications. The services that one process observes within “/svc” may or may
not match what other processes see, and can be restricted or redirected
according to application-launching policy.

For more detail the mechanisms and policies applied to restricting process
capability, refer to the documentation on
[sandboxing](/sandboxing.md).

### Passing Data

Once a connection has been established, either to a file, directory, device,
or service, subsequent operations are also transmitted using RPC messages.
These messages are transmitted on one or more handles, using a wire format that
the server validates and understands.

In the case of files, directories, and devices, these operations use the
RemoteIO protocol; in the case of services, these operations use the FIDL2
protocol, though there are plans to unify all operations into the FIDL protocol.

As an example, to seek within a file, a client would send an `ZXRIO_SEEK`
message with the desired position and “whence” within the RIO message, and the
new seek position would be returned. To truncate a file, an `FDIO_TRUNCATE`
message could be sent with the new desired filesystem, and a status message
would be returned. To read a directory, an `ZXRIO_READDIR` message could be
sent, and a list of direntries would be returned. If these requests were sent to
a filesystem entity that can’t handle them, an error would be sent, and the
operation would not be executed (like an `ZXRIO_READDIR` message sent to a text
file).

### Memory Mapping

For filesystems capable of supporting it, memory mapping files is slightly more
complicated. To actually “mmap” part of a file, a client sends an “FDIO_MMAP”
message, and receives a Virtual Memory Object, or VMO, in response. This object
is then typically mapped into the client’s address space using a Virtual Memory
Address Region, or VMAR. Transmitting a limited view of the file’s internal
“VMO” back to the client requires extra work by the intermediate message
passing layers, so they can be aware they’re passing back a server-vendored
object handle.

By passing back these virtual memory objects, clients can quickly access the
internal bytes representing the file without actually undergoing the cost of a
round-trip IPC message. This feature makes mmap an attractive option for
clients attempting high-throughput on filesystem interaction.

At the time of writing, on-demand paging is not supported by the
kernel, and has not been wired into filesystems. As a result, if a client
writes to a “memory-mapped” region, the filesystem cannot reasonably identify
which pages have and have not been touched. To cope with this restriction, mmap
has only been implemented on **read-only filesystems**, such as blobfs.

### Other Operations acting on paths

In addition to the “open” operation, there are a couple other path-based
operations worth discussing: “rename” and “link”. Unlike “open”, these
operations actually act on multiple paths at once, rather than a single
location. This complicates their usage: if a call to “rename(‘/foo/bar’,
‘baz’)” is made, the filesystem needs to figure out a way to:

  * Traverse both paths, even when they have distinct starting points (which is the
    case this here; one path starts at root, and other starts at the CWD)
  * Open the parent directories of both paths
  * Operate on both parent directories and trailing pathnames simultaneously

To satisfy this behavior, the VFS layer takes advantage of a Zircon concept
called “cookies”. These cookies allow client-side operations to store open
state on a server, using a handle, and refer to it later using that same
handles. Fuchsia filesystems use this ability to refer to one Vnode while
acting on the other.

These multi-path operations do the following:
  * Open the parent source vnode (for “/foo/bar”, this means opening “/foo”)
  * Open the target parent vnode (for “baz”, this means opening the current
    working directory) and acquire a vnode token using the operation
    `IOCTL_DEVMGR_GET_TOKEN`, which is a handle to a filesystem cookie.
  * Send a “rename” request to the source parent vnode, along with the source
    and destination paths (“bar” and “baz”), along with the vnode token acquired
    earlier. This provides a mechanism for the filesystem to safely refer to the
    destination vnode indirectly -- if the client provides an invalid handle, the
    kernel will reject the request to access the cookie, and the server can return
    an error.

## Filesystem Lifecycle

### Mounting

When Fuchsia filesystems are initialized, they are created with typically two
handles: One handle to a channel used to communicate with the mounting
filesystem (referred to as the “mount point” channel -- the “mounting” end of
this channel is saved as a field named “remote” in the parent Vnode, the other
end will be connected to the root directory of the new filesystem), and
(optionally) another to contact the underlying [block device](block_devices.md).
Once a filesystem has been initialized (reading initial state off the block
device, finding the root vnode, etc) it flags a signal (`ZX_USER_SIGNAL0`) on
the mount point channel. This informs the parent (mounting) system that the
child filesystem is ready to be utilized. At this point, the channel passed to
the filesystem on initialization may be used to send filesystem requests, such
as “open”.

At this point, the parent (mounting) filesystem “pins” the connection to the
remote filesystem on a Vnode. The VFS layers capable of path walking check for
this remote handle when observing Vnodes: if a remote handle is detected, then
the incoming request (open, rename, etc) is forwarded to the remote filesystem
instead of the underlying node. If a user actually wants to interact with the
mountpoint node, rather than the remote filesystem, they can pass the
`O_NOREMOTE` flag to the “open” operation identify this intention.

Unlike many other operating systems, the notion of “mounted filesystems” does
not live in a globally accessible table. Instead, the question “what
mountpoints exist?” can only be answered on a filesystem-specific basis -- an
arbitrary filesystem may not have access to the information about what
mountpoints exist elsewhere.

## Current Filesystems

Due to the modular nature of Fuchsia’s architecture, it is straightforward to
add filesystems to the system. At the moment, a handful of filesystems exist,
intending to satisfy a variety of distinct needs.

### MemFS: An in-memory filesystem

[MemFS](https://fuchsia.googlesource.com/zircon/+/master/system/ulib/memfs)
is used to implement requests to temporary filesystems like `/tmp`, where files
exist entirely in RAM, and are not transmitted to an underlying block device.
This filesystem is also currently used for the “bootfs” protocol, where a
large, read-only VMO representing a collection of files and directories is
unwrapped into user-accessible Vnodes at boot (these files are accessible in
`/boot`).

### MinFS: A persistent filesystem

[MinFS](https://fuchsia.googlesource.com/zircon/+/master/system/uapp/minfs/)
is a simple, traditional filesystem which is capable of storing files
persistently. Like MemFS, it makes extensive use of the VFS layers mentioned
earlier, but unlike MemFS, it requires an additional handle to a block device
(which is transmitted on startup to a new MinFS process). For ease of use,
MinFS also supplies a variety of tools: “mkfs” for formatting, “fsck” for
verification, as well as “mount” and “umount” for adding and subtracting MinFS
filesystems to a namespace from the command line.

### Blobfs: An immutable, integrity-verifying package storage filesystem

[Blobfs](https://fuchsia.googlesource.com/zircon/+/master/system/uapp/blobfs/)
is a simple, flat filesystem optimized for “write-once, then read-only” [signed
data](merkleroot.md), such as [application packages](package_metadata.md).
Other than two small prerequisites (file names which are deterministic, content
addressable hashes of a file’s Merkle Tree root, for integrity-verification)
and forward knowledge of file size (identified to Blobfs by a call to
“ftruncate” before writing a blob to storage), Blobfs appears like a
typical filesystem. It can be mounted and unmounted, it appears to contain a
single flat directory of hashes, and blobs can be accessed by operations like
“open”, “read”, “stat” and “mmap”.

### ThinFS: A FAT filesystem written in Go

[ThinFS](https://fuchsia.googlesource.com/garnet/+/master/go/src/thinfs/) is an implementation of a
FAT filesystem in Go. It serves a dual purpose: first, proving that our system
is actually modular, and capable of using novel filesystems, regardless of
language or runtime. Secondly, it provides a mechanism for reading a universal
filesystem, found on EFI partitions and many USB sticks.

### FVM

TODO: smklein