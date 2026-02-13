# S-CORE Bazel Platforms

This repository serves as the central source for S-CORE Bazel platforms and constraints, designed to:

- Provide global constraints that complement the standard set offered by Bazel, enabling more precise environment description and selection.
- Define consistent platform configurations to make toolchain selection seamless and uniform across all S-CORE modules.

By using this repository, S-CORE teams can ensure compatibility, reproducibility, and clarity when specifying build environments and extending Bazel’s default capabilities.

## Table of content

- [How to Use It](#how-to-use-it)
- [Existing Constraints](#existing-constraints)
  - [CPU Architecture](#cpu-architecture)
  - [Operating System](#operating-system)
  - [Versions](#versions)
  - [Runtime Ecosystem](#runtime-ecosystem)
- [Change Process](#change-process)
  - [Adding a New constraint_setting](#adding-a-new-constraint_setting)
  - [Adding a New constraint_value](#adding-a-new-constraint_value)
  - [Adding a New platform](#adding-a-new-platform)

## How to use it

To use predefined platforms and constraints, add `score_bazel_platforms` as a dependency in your `MODULE.bazel` file:

```python
bazel_dep(name = "score_bazel_platforms", version = "<latest-version>")
```

You can then load and reference the provided constraints or platforms in your BUILD or .bzl files.
For example, to select a platform in your .bazelrc:

```python
# in .bazelrc selecting the platform
build:<config> --platforms=@score_bazel_platforms//:x86_64-linux
```

or setting select statment based on Operating System:

```python
my_rule(
    name = "example",
    data = select({
        "@platforms//os:linux": ":linux_runtime_config",
        "@platforms//os:qnx": ":qnx_runtime_config",
    })
)
```

> NOTE: Predefined `constraint_setting` and `constraint_value` like CPU and/or OS targets can be used directly by referencing the `@platforms` module.

## Existing constraints

Platforms are typically defined by combination of operating system (OS) and CPU architecture. Our goal is to reuse Bazel's canonical definitions whenever possible, leveraging it's core platform model to ensure consistency.

Defined `constraint_settings`:

- CPU Architecture
- Operating System
- Version (OS, GCC, glibc, ...)
- Runtime ecosystem

### CPU Architecture

This setting is entirely reused from Bazel upstream definitions, however, it's important to note that the specificity of constraint_values for CPU architecture can vary. In this platform module, we only refer to the ISA (Instruction Set Architecture) - such as x86_64 or arm64 - rather than microarchitecture used in some Bazel projects.

### Operating System

This setting is also entirely reused from Bazel upstream definitions.

### Platform Variant (Kind and Version)

The platform_variant constraint represents the `<variant_kind>_<variant_version>` segment of the platform identifier:
```bash
cpu-os-<variant_kind>_<variant_version>-runtime_es
```

This constraint models a versioned environment component that influences build behavior, ABI compatibility, or runtime expectations.</br>
Only one platform variant is active per platform instance.</br>
The variant kind determines which component family is being constrained, and the variant version specifies the exact version.</br>

We currently support the following variant kinds:

- GCC version
- GLIBC version
- OS version
- SDK version
- SDP version

#### `os_version` constraint

The `os_version` constraint identifies the version of the operating system userland and ABI targeted by a platform.</br>
It captures version-specific compatibility requirements beyond the OS family itself, such as:

- kernel or userland ABI expectations
- system library versions
- OS-level feature availability
- distribution-specific behavior

This constraint allows targets to distinguish between different versions of the same operating system (for example different QNX or Linux releases) while keeping the OS family modeled separately via the os constraint.</br>
The os_version constraint is used to:

- ensure ABI compatibility
- select version-specific dependencies
- avoid overloading OS or toolchain definitions
- isolate distribution-specific patches or behaviors

#### `gcc_version` constraint

The `gcc_version` constraint identifies the compiler version used to build a target.</br>
It captures compiler-specific compatibility concerns such as:

- supported language standards
- code generation behavior
- ABI or runtime library differences
- compiler-specific workarounds or flags

This constraint allows platforms and toolchains to express compiler version requirements independently of the operating system, CPU architecture, or SDK version.</br>
The gcc_version constraint is used to:

- select appropriate toolchains
- gate compiler-dependent features
- maintain compatibility across compiler upgrades
- prevent accidental mixing of incompatible compiler runtimes

#### `glibc_version` constraint

The `glibc_version` constraint identifies the GNU C Library version targeted by the platform.</br>
Since glibc defines critical parts of the userspace ABI, this constraint directly impacts binary compatibility.</br>
It captures compatibility requirements such as:

- symbol versioning
- dynamic loader behavior
- system call wrappers
- threading and memory model behavior

This constraint is especially important when:
- building binaries intended to run on older distributions
- maintaining forward/backward ABI compatibility
- cross-compiling against a specific sysroot

The glibc_version constraint is used to:

- guarantee runtime compatibility
- prevent linking against incompatible libc versions
- enforce consistent sysroot usage

#### `sdk_version` constraint

The `sdk_version` constraint identifies the version of the Software Development Kit used to build the target.</br>
An SDK may bundle:

- compilers and linkers
- sysroots
- headers and libraries
- platform configuration files
- auxiliary build tools

The SDK version often defines a coherent and validated build environment.</br>
The sdk_version constraint is used to:

- ensure build reproducibility
- select consistent toolchain bundles
- align development and CI environments
- control transitions between SDK releases

#### `sdp_version` constraint

The `sdp_version` constraint identifies the version of the Software Distribution Platform (SDP) targeted by the build.</br>
An SDP typically represents a higher-level integration layer that may include:

- prebuilt components
- distribution packaging rules
- runtime layout definitions
- platform integration policies

Unlike SDK, which is primarily a build-time concern, SDP may affect both build-time integration and runtime deployment structure.</br>
The sdp_version constraint is used to:

- ensure compatibility with distribution packaging rules
- align with deployment/runtime expectations
- control integration against specific platform releases

### Runtime Ecosystem

The runtime_ecosystem constraint identifies the system-level runtime environment a binary or target is built to run in.</br>
It captures non-OS, non-CPU assumptions about the execution environment, such as:
- available system services and middleware
- process lifecycle and supervision
- IPC and communication models
- deployment and startup conventions

This constraint distinguishes mutually exclusive runtime ecosystems (for example AutoSD, Android Automotive, or pure POSIX) that may run on the same operating system and CPU architecture but expose different runtime contracts.</br>
The runtime_ecosystem constraint is used for:
- platform compatibility checks
- conditional dependencies via select()
- expressing runtime-specific behavior without overloading OS or CPU constraints

Currently, our platform support following `runtime_es`:
- `autosd`: is an automotive runtime platform and software ecosystem that defines a standardized execution environment for in-vehicle applications. From a build and platform perspective, AutoSD represents a distinct runtime ecosystem that is orthogonal to OS and CPU architecture and must be modeled explicitly to ensure correct compatibility and dependency selection.
- `posix` : denotes a generic execution environment where applications rely only on standardized POSIX APIs and behavior, without assumptions about higher-level middleware, platform-specific services, or vendor-defined runtime frameworks.
- `ebclfsa`: EB corbos Linux for Safety Applications is a hypervisor-based safety extension to Linux. It can be used for automotive safety-relevant workloads (up to ASIL B / SIL 2 level under ISO 26262 and IEC 61508 standards).

## Change process

### Adding a New constraint_setting

1. Create a new constraint_setting target in an appropriate package or directory.
2. Document its purpose clearly in the `BUILD` file.
3. Open a pull request with:
   - The new `constraint_setting`
   - A brief explanation of why it's needed and how it should be used

### Adding a New constraint_value

1. Define a new constraint_value under the relevant constraint_setting.
2. Ensure it’s logically named and documented.
3. Submit a pull request with the change and a short description.

Once reviewed and approved, the new constraint will be available for use across all dependent modules.

### Adding a New platform

This repository defines canonical Bazel platforms that combine existing constraint values into a single, reusable target. Platforms must follow a strict naming convention to ensure consistency, discoverability, and predictable toolchain selection across all S-CORE modules.

#### Platform Naming Convention

Each platform must be named according to the following format:
```bash
{target_cpu}-{target_os}-{gcc_version|sdp_version|or other version}-{runtime_es}
```
Where:
| Component           | Description                                  | Source                        |
|---------------------|----------------------------------------------|-------------------------------|
| `target_cpu`        | CPU instruction set architecture             | `@platforms//cpu`             |
| `target_os`         | Operating system family                      | `@platforms//os`              |
| `platform_variant`  | GCC version or SDK/SDP version or OS version | `gcc_version` or `os_version` |
| `runtime_es`        | Runtime ecosystem                            | `runtime_es`                  |

Note that 3rd and 4th segment are optional to set (however we advise people to use it).

Examples:
| Platform name                  | Meaning           |
|--------------------------------|-----------------------------------------------------------------------------------|
| x86_64-linux-gcc_12.2.0-posix  | x86_64 Linux built with GCC 12.2.0, POSIX runtime - generic execution environment |
| aarch64-linux-autosd10         | Arm 64 Linux built, AutoSD 10 runtime - Automotive Stream Distribution            |
| x86_64-linux-sdk_0.1.0-ebclfsa | x86_64 Linux built, EBcLfSA runtime - EB corbos Linux for Safety Applications     |


#### Naming Rules (Mandatory)

1. **OS and CPU are mandatory, platform_variant and runtime_es are optional **.</br>
No component may be omitted or merged with another.

2. **Lowercase only**.</br>
Platform names must be lowercase and kebab-cased.

3. **Exactly one version dimension**.</br>
Use:
    - gcc_version for generic Linux / POSIX builds
    - os_version or SDK/SDP version for vendor OS platforms (e.g. QNX)

4. **Runtime ecosystem is always explicit**.</br>
Even for “generic” environments, posix must be stated explicitly.

5. **No ambiguity**.</br>
The platform name alone must uniquely describe the execution and toolchain environment without additional context.

#### Design Rationale

This strict platform naming scheme ensures:
- Deterministic toolchain selection
- Clear ABI and runtime expectations
- Predictable select() behavior
- Long-term maintainability as platforms scale

Any deviation from this convention must be explicitly justified and approved during review.
