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

### Versions

Version as constraint is actually reserved for multiple constraint. We currently support:
- OS version
- GCC version

#### `os_version` constraint

The os_version constraint identifies the version of the operating system userland and ABI targeted by a platform.</br>
It captures version-specific compatibility requirements beyond the OS family itself, such as:
- kernel or userland ABI expectations
- system library versions
- vendor SDK or BSP alignment
- OS-level feature availability

This constraint allows targets to distinguish between different versions of the same operating system (for example different QNX or Linux releases) while keeping the OS family modeled separately via the os constraint.</br>
The os_version constraint is used to:
- ensure ABI compatibility
- select version-specific dependencies
- avoid overloading OS or toolchain definitions

#### `gcc_version` constraint

The gcc_version constraint identifies the major compiler version used to build a target.</br>
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

Each platform MUST be named according to the following format:
```bash
{target_cpu}-{target_os}-{gcc_version|sdp_version|or other version}-{runtime_es}
```
Where:
| Component           | Description                                  | Source                        |
|---------------------|----------------------------------------------|-------------------------------|
| `target_cpu`        | CPU instruction set architecture             | `@platforms//cpu`             |
| `target_os`         | Operating system family                      | `@platforms//os`              |
| `version`           | GCC version or SDK/SDP version or OS version | `gcc_version` or `os_version` |
| `runtime_es`        | Runtime ecosystem                            | `runtime_es`                  |

Examples:
| Platform name                 | Meaning           |
|-------------------------------|-----------------------------------------------------------------------------------|
| x86_64-linux-gcc_12.2.0-posix | x86_64 Linux built with GCC 12.2.0, POSIX runtime - generic execution environment |

#### Naming Rules (Mandatory)

1. **All four components are required**.</br>
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
