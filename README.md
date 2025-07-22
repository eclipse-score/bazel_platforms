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
- [Change Process](#change-process)
  - [Adding a New constraint_setting](#adding-a-new-constraint_setting)
  - [Adding a New constraint_value](#adding-a-new-constraint_value)

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

### CPU Architecture

This setting is entirely reused from Bazel upstream definitions, however, it's important to note that the specificity of constraint_values for CPU architecture can vary. In this platform module, we only refer to the ISA (Instruction Set Architecture) - such as x86_64 or arm64 - rather than microarchitecture used in some Bazel projects.

### Operating System

This setting is also entirely reused from Bazel upstream definitions.

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
