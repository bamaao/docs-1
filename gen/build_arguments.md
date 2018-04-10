# Build Arguments
### always_zedboot
    Build boot images that prefer Zedboot over local boot.

#### Default Value
false

### arm_float_abi
    The ARM floating point mode. This is either the string "hard", "soft", or
    "softfp". An empty string means to use the default one for the
    arm_version.

#### Default Value
""

### arm_optionally_use_neon
    Whether to enable optional NEON code paths.

#### Default Value
false

### arm_tune
    The ARM variant-specific tuning mode. This will be a string like "armv6"
    or "cortex-a15". An empty string means to use the default for the
    arm_version.

#### Default Value
""

### arm_use_neon
    Whether to use the neon FPU instruction set or not.

#### Default Value
true

### arm_version

#### Default Value
8

### armadillo_path_context_config

#### Default Value
"armadillo_user_shell/assets/contextual_config.json"

### bootfs_packages
    Build packages as FAR archives and store them in /system. This is required
    for --bootfs builds.

#### Default Value
false

### build_intel_gen

#### Default Value for x64
true

#### Default Value for arm64
false

### build_libvulkan_arm_mali
    This is a list of targets that will be built as drivers. If more than one
    target is given then use_vulkan_loader_for_tests must be set to true, as
    otherwise tests won't know which libvulkan to use. Example gn arg:
    build_libvulkan_arm_mali =
    ["//third_party/arm-mali-bifrost:libvulkan_arm"]

#### Default Value
[]

### build_msd_arm_mali

#### Default Value for x64
false

#### Default Value for arm64
true

### clang_prefix

#### Default Value
"../../../buildtools/linux-x64/clang/bin"

### current_cpu

#### Default Value
""

### current_os

#### Default Value
""

### dart1_default_app

#### Default Value
"dart1_jit_app"

### dart2_default_app

#### Default Value
"dart2_jit_app"

### dart_core_snapshot_kind
    Controls the kind of core snapshot linked into the standalone VM. Using a
    core-jit snapshot breaks the ability to change various flags that affect
    code generation.

#### Default Value
"core"

### dart_custom_version_for_pub
    When this argument is a non-empty string, the version repoted by the
    Dart VM will be one that is compatible with pub's interpretation of
    semantic version strings. The version string will also include the values
    of the argument. In particular the version string will read:
   
        "M.m.p-dev.x.x-$(dart_custom_version_for_pub)-$(short_git_hash)"
   
    Where 'M', 'm', and 'p' are the major, minor and patch version numbers,
    and 'dev.x.x' is the dev version tag most recently preceeding the current
    revision. The short git hash can be omitted by setting
    dart_version_git_info=false

#### Default Value
""

### dart_debug
    Instead of using is_debug, we introduce a different flag for specifying a
    Debug build of Dart so that clients can still use a Release build of Dart
    while themselves doing a Debug build.

#### Default Value
false

### dart_debug_optimization_level
    The optimization level to use for debug builds.

#### Default Value
"2"

### dart_default_app

#### Default Value
"dart2_jit_app"

### dart_platform_sdk
    Build a SDK with less stuff. It excludes dart2js, ddc, and web libraries.

#### Default Value
true

### dart_runtime_mode
    Set the runtime mode. This affects how the runtime is built and what
    features it has. Valid values are:
    'develop' (the default) - VM is built to run as a JIT with all development
    features enabled.
    'profile' - The VM is built to run with AOT compiled code with only the
    CPU profiling features enabled.
    'release' - The VM is built to run with AOT compiled code with no developer
    features enabled.
   
    These settings are only used for Flutter, at the moment. A standalone build
    of the Dart VM should leave this set to "develop", and should set
    'is_debug', 'is_release', or 'is_product'.
   
    TODO(rmacnak): dart_runtime_mode no longer selects whether libdart is build
    for JIT or AOT, since libdart waw split into libdart_jit and
    libdart_precompiled_runtime. We should remove this flag and just set
    dart_debug/dart_product.

#### Default Value
"develop"

### dart_snapshot_kind
    Default to building app-jit snapshots. The simulator and cross builds
    override this to script snapshots to cut down on build time.

#### Default Value for x64
"app-jit"

#### Default Value for arm64
"script"

### dart_stripped_binary
    Path to stripped dart binary relative to build output directory.

#### Default Value
"dart"

### dart_target_arch
    Explicitly set the target architecture to use a simulator.
    Available options are: arm, arm64, x64, ia32, and dbc.

#### Default Value for x64
"x64"

#### Default Value for arm64
"arm64"

### dart_use_fallback_root_certificates
    Whether to fall back to built-in root certificates when they cannot be
    verified at the operating system level.

#### Default Value
false

### dart_use_tcmalloc
    Whether to link the standalone VM against tcmalloc. The standalone build of
    the VM enables this only for Linux builds.

#### Default Value
false

### dart_version_git_info
    Whether the Dart binary version string should include the git hash and
    git commit time.

#### Default Value
true

### dart_zlib_path
    The BUILD.gn file that we pull from chromium as part of zlib has a
    dependence on //base, which we don't pull in. In a standalone build of the
    VM, we set this to //runtime/bin/zlib where we have a BUILD.gn file without
    a dependence on //base.

#### Default Value
"//third_party/zlib"

### data_image_size
    The size of the minfs data partition image to create. Normally this image
    is added to FVM, and can therefore expand as needed. It must be at least
    10mb (the default) in order to be succesfully initialized.

#### Default Value
"10m"

### enable_dummy_subsystem

#### Default Value
true

### enable_gfx_subsystem

#### Default Value
true

### enable_sketchy_subsystem

#### Default Value
true

### enable_value_subsystem

#### Default Value
false

### enable_views_subsystem

#### Default Value
true

### expat_build_root

#### Default Value
"//third_party/expat"

### experimental_web_entity_extraction

#### Default Value
false

### extra_authorized_keys_file
    Additional SSH authorized_keys file to include in the build.
    For example:
      extra_authorized_keys_file=\"$HOME/.ssh/id_rsa.pub\"

#### Default Value
""

### extra_variants
    Additional variant toolchain configs to support.
    This is just added to `known_variants`, which see.

#### Default Value
[]

### ffmpeg_profile

#### Default Value for x64
"max"

#### Default Value for arm64
"default"

### flutter1_default_app

#### Default Value
"flutter1_jit_app"

### flutter2_default_app

#### Default Value
"flutter2_jit_app"

### flutter_aot
    Enable ahead-of-time compilation on platforms where AOT is optional.

#### Default Value
false

### flutter_asserts_always_on
    Enable asserts, even in release builds.

#### Default Value
false

### flutter_default_app

#### Default Value
"flutter1_jit_app"

### flutter_runtime_mode
    The runtime mode ("debug", "profile", or "release")

#### Default Value
"debug"

### fuchsia_packages
    List of packages (a comma-separated string).  If unset, guessed based
    on which layer is found in the //.jiri_manifest file.

#### Default Value
[]

### fuchsia_use_vulkan
    Consolidated build toggle for use of Vulkan across Fuchsia

#### Default Value
true

### fuchsia_vulkan_sdk
    Path to Fuchsia Vulkan SDK

#### Default Value
"//third_party/vulkan_loader_and_validation_layers"

### glm_build_root

#### Default Value
"//third_party/glm"

### goma_dir
    Absolute directory containing the Goma source code.

#### Default Value
"/usr/local/google/home/juliehockett/goma"

### host_byteorder

#### Default Value
"undefined"

### host_cpu

#### Default Value
"x64"

### host_os

#### Default Value
"linux"

### host_tools_dir
    This is the directory where host tools intended for manual use by
    developers get installed.  It's something a developer might put
    into their shell's $PATH.  Host tools that are just needed as part
    of the build do not get copied here.  This directory is only for
    things that are generally useful for testing or debugging or
    whatnot outside of the GN build itself.  These are only installed
    by an explicit install_host_tools() rule (see //build/host.gni).

#### Default Value
"//topaz/out/Default/tools"

### icu_use_data_file
    Tells icu to load an external data file rather than rely on the icudata
    being linked directly into the binary.
   
    This flag is a bit confusing. As of this writing, icu.gyp set the value to
    0 but common.gypi sets the value to 1 for most platforms (and the 1 takes
    precedence).
   
    TODO(GYP) We'll probably need to enhance this logic to set the value to
    true or false in similar circumstances.

#### Default Value
true

### is_debug
    Debug build.

#### Default Value
true

### kernel_cmdline_file
    File containing kernel command line arguments to roll into the
    bootdata image used for booting.

#### Default Value
""

### known_variants
    List of variants that will form the basis for variant toolchains.
    To make use of a variant, set `select_variant` (which see).
   
    Normally this is not set as a build argument, but it serves to
    document the available set of variants.  See also `universal_variants`.
    Only set this to remove all the default variants here.
    To add more, set `extra_variants` instead.
   
    Each element of the list is one variant, which is a scope defining:
   
      configs (optional)
          [list of labels] Each label names a config that will be
          automatically used by every target built in this variant.
          For each config ${label}, there must also be a target
          ${label}_deps, which each target built in this variant will
          automatically depend on.  The `variant()` template is the
          recommended way to define a config and its `_deps` target at
          the same time.
   
      remove_common_configs (optional)
      remove_shared_configs (optional)
          [list of labels] This list will be removed (with `-=`) from
          the `default_common_binary_configs` list (or the
          `default_shared_library_configs` list, respectively) after
          all other defaults (and this variant's configs) have been
          added.
   
      deps (optional)
          [list of labels] Added to the deps of every target linked in
          this variant (as well as the automatic "${label}_deps" for
          each label in configs).
   
      name (required if configs is omitted)
          [string] Name of the variant as used in `select_variant`
          elements' `variant` fields.  It's a good idea to make it
          something concise and meaningful when seen as e.g. part of a
          directory name under `$root_build_dir`.  If name is omitted,
          configs must be nonempty and the simple names (not the full
          label, just the part after all /s and :s) of these configs
          will be used in toolchain names (each prefixed by a "-"), so
          the list of config names forming each variant must be unique
          among the lists in `known_variants + extra_variants`.
   
      toolchain_args (optional)
          [scope] Each variable defined in this scope overrides a
          build argument in the toolchain context of this variant.
   
      host_only (optional)
      target_only (optional)
          [scope] This scope can contain any of the fields above.
          These values are used only for host or target, respectively.
          Any fields included here should not also be in the outer scope.
   

#### Default Value
[{
  configs = ["//build/config/lto"]
}, {
  configs = ["//build/config/lto:thinlto"]
}, {
  configs = ["//build/config/scudo"]
}, {
  configs = ["//build/config/sanitizers:ubsan"]
}, {
  configs = ["//build/config/sanitizers:ubsan", "//build/config/sanitizers:sancov"]
}, {
  configs = ["//build/config/sanitizers:asan"]
  host_only = {
  remove_shared_configs = ["//build/config:symbol_no_undefined"]
}
}, {
  configs = ["//build/config/sanitizers:asan", "//build/config/sanitizers:sancov"]
  host_only = {
  remove_shared_configs = ["//build/config:symbol_no_undefined"]
}
}, {
  name = "asan_no_detect_leaks"
  toolchain_args = {
  asan_default_options = "detect_leaks=0"
}
  configs = ["//build/config/sanitizers:asan"]
  host_only = {
  remove_shared_configs = ["//build/config:symbol_no_undefined"]
}
}]

### magma_build_root

#### Default Value
"//garnet/lib/magma"

### magma_enable_developer_build
    Enable this to have the msd include a suite of tests and invoke them
    automatically when the driver starts.

#### Default Value
false

### magma_enable_tracing
    Enable this to include fuchsia tracing capability

#### Default Value
true

### magma_python_path

#### Default Value
"/usr/local/google/home/juliehockett/fuchsia/third_party/mako"

### mesa_build_root

#### Default Value
"//third_party/mesa"

### msd_arm_enable_all_cores
    Enable all 8 cores, which is faster but emits more heat.

#### Default Value
true

### msd_arm_enable_cache_coherency
    With this flag set the system tries to use cache coherent memory if the
    GPU supports it.

#### Default Value
true

### msd_intel_enable_mapping_cache

#### Default Value
false

### msd_intel_enable_modesetting

#### Default Value
false

### msd_intel_gen_build_root

#### Default Value
"//garnet/drivers/gpu/msd-intel-gen"

### msd_intel_print_fps

#### Default Value
false

### msd_intel_reported_height

#### Default Value
0

### msd_intel_reported_width

#### Default Value
0

### msd_intel_wait_for_flip

#### Default Value
true

### prebuilt_libvulkan_arm_path

#### Default Value
""

### rustc_prefix
    Sets a custom base directory for `rustc` and `cargo`.
    This can be used to test custom Rust toolchains.

#### Default Value
"//buildtools/linux-x64/rust/bin"

### scene_manager_vulkan_swapchain
    0 - use normal swapchain
    1 - use vulkan swapchain, but wait for real display
    2 - use vulkan swapchain with fixed-size fake display

#### Default Value
0

### scudo_default_options
    Default [Scudo](https://llvm.org/docs/ScudoHardenedAllocator.html)
    options (before the `SCUDO_OPTIONS` environment variable is read at
    runtime).  *NOTE:* This affects only components using the `scudo`
    variant (see GN build argument `select_variant`), and does not affect
    anything when the `use_scudo` build flag is set instead.

#### Default Value
["abort_on_error=1", "QuarantineSizeKb=0", "ThreadLocalQuarantineSizeKb=0"]

### sdk_dirs
    The directories to search for parts of the SDK.
   
    By default, we search the public directories for the various layers.
    In the future, we'll search a pre-built SDK as well.

#### Default Value
["//garnet/public", "//peridot/public", "//topaz/public"]

### select_variant
    List of "selectors" to request variant builds of certain targets.
    Each selector specifies matching criteria and a chosen variant.
    The first selector in the list to match a given target determines
    which variant is used for that target.
   
    Each selector is either a string or a scope.  A shortcut selector is
    a string; it gets expanded to a full selector.  A full selector is a
    scope, described below.
   
    A string selector can match a name in `select_variant_shortcuts`,
    which see.  If it's not a specific shortcut listed there, then it
    can be the name of any variant described in `known_variants` and
    `universal_variants` (and combinations thereof).  A `selector`
    that's a simple variant name selects for every binary built in the
    target toolchain: `{ host=false variant=selector }`.
   
    If a string selector contains a slash, then it's `"shortcut/filename"`
    and selects only the binary in the target toolchain whose `output_name`
    matches `"filename"`, i.e. it adds `output_name=["filename"]` to each
    selector scope that the shortcut's name alone would yield.
   
    The scope that forms a full selector defines some of these:
   
        variant (required)
            [string or false] The variant that applies if this selector
            matches.  This can be false to choose no variant, or a
            string that names the variant.  See `known_variants` and
            `universal_variants`.
   
    The rest below are matching criteria.  All are optional.
    The selector matches if and only if all of its criteria match.
    If none of these is defined, then the selector always matches.
   
    The first selector in the list to match wins and then the rest of
    the list is ignored. So construct more complex rules by using a
    "blacklist" selector with `variant=false` before a catch-all or
    "whitelist" selector that names a variant.
   
    Each "[strings]" criterion is a list of strings, and the criterion
    is satisfied if any of the strings matches against the candidate string.
   
        host
            [boolean] If true, the selector matches in the host toolchain.
            If false, the selector matches in the target toolchain.
   
        testonly
            [boolean] If true, the selector matches targets with testonly=true.
            If false, the selector matches in targets without testonly=true.
   
        target_type
            [strings]: "executable", "loadable_module", or "driver_module"
   
        output_name
            [strings]: target's `output_name` (default: its `target name`)
   
        label
            [strings]: target's full label with `:` (without toolchain suffix)
   
        name
            [strings]: target's simple name (label after last `/` or `:`)
   
        dir
            [strings]: target's label directory (`//dir` for `//dir:name`).

#### Default Value
[]

### select_variant_canonical
    *This should never be set as a build argument.*
    It exists only to be set in `toolchain_args`.
    See //build/toolchain/clang_toolchain.gni for details.

#### Default Value
[]

### select_variant_shortcuts
    List of short names for commonly-used variant selectors.  Normally this
    is not set as a build argument, but it serves to document the available
    set of short-cut names for variant selectors.  Each element of this list
    is a scope where `.name` is the short name and `.select_variant` is a
    a list that can be spliced into `select_variant`, which see.

#### Default Value
[{
  select_variant = [{
  variant = "asan_no_detect_leaks"
  host = true
  dir = ["//third_party/yasm", "//third_party/vboot_reference", "//garnet/tools/vboot_reference"]
}, {
  variant = "asan"
  host = true
}]
  name = "host_asan"
}]

### shell_enable_vulkan

#### Default Value
false

### skia_android_serial

#### Default Value
""

### skia_compile_processors

#### Default Value
false

### skia_enable_atlas_text

#### Default Value
false

### skia_enable_discrete_gpu

#### Default Value
true

### skia_enable_effects

#### Default Value
true

### skia_enable_flutter_defines

#### Default Value
[false](//third_party/skia/BUILD.gn#38)

#### Current Value
[true](//.gn#27)

### skia_enable_fontmgr_empty

#### Default Value
false

### skia_enable_gpu

#### Default Value
true

### skia_enable_pdf

#### Default Value
true

### skia_enable_spirv_validation

#### Default Value
false

### skia_enable_tools

#### Default Value
false

### skia_enable_vulkan_debug_layers

#### Default Value
false

### skia_lex

#### Default Value
false

### skia_llvm_lib

#### Default Value
"LLVM"

### skia_llvm_path

#### Default Value
""

### skia_qt_path

#### Default Value
""

### skia_skqp_enable_driver_correctness_workarounds

#### Default Value
false

### skia_skqp_global_error_tolerance

#### Default Value
0

### skia_tools_vulkan_header_dir

#### Default Value
""

### skia_use_angle

#### Default Value
false

### skia_use_dng_sdk

#### Default Value
[true](//third_party/skia/BUILD.gn#58)

#### Current Value
[false](//.gn#28)

### skia_use_egl

#### Default Value
false

### skia_use_expat

#### Default Value
true

### skia_use_fontconfig

#### Default Value
[true](//third_party/skia/BUILD.gn#22)

#### Current Value
[false](//.gn#29)

### skia_use_freetype

#### Default Value
true

### skia_use_icu

#### Default Value
true

### skia_use_legacy_gpu_pixel_ops

#### Default Value
false

### skia_use_libheif

#### Default Value
false

### skia_use_libjpeg_turbo

#### Default Value
true

### skia_use_libpng

#### Default Value
true

### skia_use_libwebp

#### Default Value
[true](//third_party/skia/BUILD.gn#27)

#### Current Value
[false](//.gn#30)

### skia_use_lua

#### Default Value
false

### skia_use_metal

#### Default Value
false

### skia_use_piex

#### Default Value
true

### skia_use_sfntly

#### Default Value
[true](//third_party/skia/BUILD.gn#59)

#### Current Value
[false](//.gn#31)

### skia_use_skcms

#### Default Value
false

### skia_use_vulkan

#### Default Value
false

### skia_use_zlib

#### Default Value
true

### skia_vulkan_header

#### Default Value
""

### skia_vulkan_sdk

#### Default Value
""

### system_package_key
    The package key to use for signing the system package. New keys can be
    generated with the `pm` host command.

#### Default Value
"//build/development.key"

### target_cpu

#### Default Value
""

#### Current Value for x64
"x64"

#### Current Value for arm64
"arm64"

### target_os

#### Default Value
""

### target_sysroot
    The absolute path of the sysroot that is used with the target toolchain.

#### Default Value
""

### thinlto_cache_dir
    ThinLTO cache directory path.

#### Default Value for x64
"dart_host_x64/thinlto-cache"

#### Default Value for arm64
"host_x64/thinlto-cache"

### thinlto_jobs
    Number of parallel ThinLTO jobs.

#### Default Value
8

### toolchain_manifests
    Manifest files describing target libraries from toolchains.
    Can be either // source paths or absolute system paths.

#### Default Value for x64
["/usr/local/google/home/juliehockett/fuchsia/buildtools/linux-x64/clang/lib/x86_64-fuchsia.manifest"]

#### Default Value for arm64
["/usr/local/google/home/juliehockett/fuchsia/buildtools/linux-x64/clang/lib/aarch64-fuchsia.manifest"]

### toolchain_variant
    *This should never be set as a build argument.*
    It exists only to be set in `toolchain_args`.
    See //build/toolchain/clang_toolchain.gni for details.
    This variable is a scope giving details about the current toolchain:
        toolchain_variant.base
            [label] The "base" toolchain for this variant, *often the
            right thing to use in comparisons, not `current_toolchain`.*
            This is the toolchain actually referenced directly in GN
            source code.  If the current toolchain is not
            `shlib_toolchain` or a variant toolchain, this is the same
            as `current_toolchain`.  In one of those derivative
            toolchains, this is the toolchain the GN code probably
            thought it was in.  This is the right thing to use in a test
            like `toolchain_variant.base == target_toolchain`, rather
            rather than comparing against `current_toolchain`.
        toolchain_variant.name
            [string] The name of this variant, as used in `variant` fields in
            `select_variant` clauses.  In the base toolchain and its
            `shlib_toolchain`, this is "".
        toolchain_variant.suffix
            [string] This is "-${toolchain_variant.name}", "" if name is empty.
        toolchain_variant.is_pic_default
            [bool] This is true in `shlib_toolchain`.
    The other fields are the variant's effects as defined in `known_variants`.

#### Default Value for x64
{
  base = "//build/toolchain/fuchsia:x64"
}

#### Default Value for arm64
{
  base = "//build/toolchain/fuchsia:arm64"
}

### universal_variants

#### Default Value
[{
  toolchain_args = {
  is_debug = false
}
  configs = []
  name = "release"
}]

### use_analysis_server
    Whether to use the analysis server instead of the CLI.

#### Default Value
false

### use_ccache
    Set to true to enable compiling with ccache

#### Default Value
false

### use_frozen_with_cargo
    Allows Cargo.lock to be updated when performing local builds.

#### Default Value
true

### use_goma
    Set to true to enable distributed compilation using Goma.

#### Default Value
false

### use_lto
    Use link time optimization (LTO).

#### Default Value
false

### use_mock_magma

#### Default Value
false

### use_prebuilt_webkit
    Use a prebuilt WebKit binary rather than building it locally.
    See //topaz/runtime/web_view/README.md for details on the prebuilt.
    This is ignored when building WebKit-using components
    such as `web_view` in variant builds (e.g. sanitizers).

#### Default Value
true

### use_scudo
    Enable the [Scudo](https://llvm.org/docs/ScudoHardenedAllocator.html)
    memory allocator for Fuchsia.

#### Default Value
false

### use_thinlto
    Use ThinLTO variant of LTO if use_lto = true.

#### Default Value
true

### use_vulkan_loader_for_tests
    Mesa doesn't properly handle loader-less operation;
    their GetInstanceProcAddr implementation returns 0 for some interfaces.

#### Default Value for x64
true

#### Default Value for arm64
false

### zedboot_cmdline_file
    File containing kernel command line arguments to roll into the
    bootdata image used for zedboot.

#### Default Value
""

### zircon_asserts

#### Default Value
true

### zircon_aux_manifests
    Manifest files describing extra libraries from a Zircon build
    not included in `zircon_boot_manifests`, such as an ASan build.
    Can be either // source paths or absolute system paths.
   
    Since Zircon manifest files are relative to a Zircon source directory
    rather than to the directory containing the manifest, these are assumed
    to reside in a build directory that's a direct subdirectory of the
    Zircon source directory and thus their contents can be taken as
    relative to `get_path_info(entry, "dir") + "/.."`.
    TODO(mcgrathr): Make Zircon manifests self-relative too and then
    merge this and toolchain_manifests into generic aux_manifests.

#### Default Value for x64
["//out/build-zircon/build-x64-ulib/bootfs.manifest"]

#### Default Value for arm64
["//out/build-zircon/build-arm64-ulib/bootfs.manifest"]

### zircon_boot_groups
    Groups to include from the Zircon /boot manifest into /boot.
    This is either "all" or a comma-separated list of one or more of:
      core -- necessary to boot
      misc -- utilities in /bin
      test -- test binaries in /bin and /test

#### Default Value
"core"

### zircon_boot_manifests
    Manifest files describing files to go into the `/boot` filesystem.
    Can be either // source paths or absolute system paths.
    `zircon_boot_groups` controls which files are actually selected.
   
    Since Zircon manifest files are relative to a Zircon source directory
    rather than to the directory containing the manifest, these are assumed
    to reside in a build directory that's a direct subdirectory of the
    Zircon source directory and thus their contents can be taken as
    relative to `get_path_info(entry, "dir") + "/.."`.

#### Default Value for x64
["//out/build-zircon/build-x64/bootfs.manifest"]

#### Default Value for arm64
["//out/build-zircon/build-arm64/bootfs.manifest"]

### zircon_build_dir
    Zircon build directory for `target_cpu`, containing `.manifest` and
    `.bin` files for Zircon's BOOTFS, BOOTDATA, and kernel image.

#### Default Value for x64
"//out/build-zircon/build-x64"

#### Default Value for arm64
"//out/build-zircon/build-arm64"

### zircon_build_root

#### Default Value
"//zircon"

### zircon_system_groups
    TODO(mcgrathr): Could default to "" for !is_debug, or "production
    build".  Note including "test" here places all of Zircon's tests
    into /system/test, which means that Fuchsia bots run those tests
    too.

#### Default Value
"misc,test"

### zircon_tools_dir
    Where to find Zircon's host-side tools that are run as part of the build.

#### Default Value
"//out/build-zircon/tools"

