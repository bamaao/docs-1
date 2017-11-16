Rust Quickstart
===============

The goal of this guide is to smooth the process of using Rust on fuchsia for the first time. As such this guide will have some redundancy with other existing docs.

## Prerequisites

Before starting, this guide assumes you can successfully build and run fuchsia, either on real hardware or virtualized.
If you have not built fuchsia before, please see the [Getting Started on Fuchsia Docs](https://fuchsia.googlesource.com/docs/+/master/getting_started.md).


## Getting Rust Setup

If this is your first time using Rust, we recommend you learn a little more about the language before proceeding. We
recommend the canonical intro to Rust resource, [The Rust Programming Language Book (TRPL) ](https://doc.rust-lang.org/book/).
Alternatively, [Learning Rust With Entirely Too Many Linked Lists](http://cglab.ca/~abeinges/blah/too-many-lists/book/)
is also a good option.


The recommended way of installing rust is to use [rustup](https://rustup.rs/).

## Building a Rust Binary

There are two approaches to building a Rust binary which will run on Fuchsia.

1. Simply compiling the binary, copying it onto the running fuchsia machine, and executing it.
2. Defining the binary as a package/module using GN, and including it as a package when building fuchsia.


For the first approach, we use a tool called [Fargo](https://fuchsia.googlesource.com/fargo/), which is a convenience layer
built on top of [Cargo](http://doc.crates.io/guide.html) (Rust's package manager and build system) to make compiling binaries
targeting Fuchsia easier. See [getting started in Fargo's readme](https://fuchsia.googlesource.com/fargo/#getting-started)
for how to get started building and running Rust binaries using Fargo.

The second approach will require using GN, Fuchsia's build tool, to define Rust binaries and packages. For more on how
to define fuchsia packages written in Rust see [the fuchsia rust doc](https://fuchsia.googlesource.com/docs/+/master/rust.md).
Once you have defined your the package for your Rust binary in GN, you can simple include it when calling packages/gn/gen.py
(or fset if you're using the env.sh functions).

```sh
fx set x86-64 --release --packages garnet/packages/my_rust_package
```

Then, when you build Fuchsia, your binary will be included in the final build. Additionally, when you run Fuchsia your
binary should be copied onto the running machine.


### Common Difficulties Encountered Using Rust+GN

## Examples



## FIDL

Rust has [FIDL bindings](https://fuchsia.googlesource.com/garnet/+/master/public/lib/fidl/rust/fidl) to support writing
FIDL services in Rust. For an example of a fidl service implemented in Rust, check out Rust implementations of the
[echo server and client](https://fuchsia.googlesource.com/garnet/+/master/examples/fidl/).


## Need Help On Something Specific

* Feel free to stop by the #fuchsia-rust irc channel on freenode.
* Help with fidl:
* Help with fargo:
* Help with using Rust with GN:
* Help with third party rust libraries:

