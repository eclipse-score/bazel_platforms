# *******************************************************************************
# Copyright (c) 2025 Contributors to the Eclipse Foundation
#
# See the NOTICE file(s) distributed with this work for additional
# information regarding copyright ownership.
#
# This program and the accompanying materials are made available under the
# terms of the Apache License Version 2.0 which is available at
# https://www.apache.org/licenses/LICENSE-2.0
#
# SPDX-License-Identifier: Apache-2.0
# *******************************************************************************

""" This files holds global definitions of bazel platforms
"""

package(default_visibility = ["//visibility:public"])

# --------------------------------------------------------------------------------
# List of base platforms (OS + CPU constraint)
# --------------------------------------------------------------------------------
platform(
    name = "aarch64-linux",
    constraint_values = [
        "@platforms//cpu:aarch64",
        "@platforms//os:linux",
    ],
)

platform(
    name = "aarch64-qnx",
    constraint_values = [
        "@platforms//cpu:aarch64",
        "@platforms//os:qnx",
    ],
)

platform(
    name = "x86_64-linux",
    constraint_values = [
        "@platforms//cpu:x86_64",
        "@platforms//os:linux",
    ],
)

platform(
    name = "x86_64-qnx",
    constraint_values = [
        "@platforms//cpu:x86_64",
        "@platforms//os:qnx",
    ],
)

# --------------------------------------------------------------------------------
# List of derived platforms from base platforms
# --------------------------------------------------------------------------------
platform(
    name = "aarch64-qnx-sdp_8.0.0-posix",
    constraint_values = [
        "@score_bazel_platforms//version:sdp_8.0.0",
        "@score_bazel_platforms//runtime_es:posix",
    ],
    parents = [":aarch64-qnx"],
)

platform(
    name = "aarch64-qnx-sdp_7.1.0-posix",
    constraint_values = [
        "@score_bazel_platforms//version:sdp_7.1.0",
        "@score_bazel_platforms//runtime_es:posix",
    ],
    parents = [":aarch64-qnx"],
)

platform(
    name = "x86_64-qnx-sdp_8.0.0-posix",
    constraint_values = [
        "@score_bazel_platforms//version:sdp_8.0.0",
        "@score_bazel_platforms//runtime_es:posix",
    ],
    parents = [":x86_64-qnx"],
)

platform(
    name = "x86_64-qnx-sdp_7.1.0-posix",
    constraint_values = [
        "@score_bazel_platforms//version:sdp_7.1.0",
        "@score_bazel_platforms//runtime_es:posix",
    ],
    parents = [":x86_64-qnx"],
)

platform(
    name = "aarch64-linux-gcc_12.2.0-posix",
    constraint_values = [
        "@score_bazel_platforms//version:gcc_12.2.0",
        "@score_bazel_platforms//runtime_es:posix",
    ],
    parents = [":aarch64-linux"],
)

platform(
    name = "x86_64-linux-gcc_12.2.0-posix",
    constraint_values = [
        "@score_bazel_platforms//version:gcc_12.2.0",
        "@score_bazel_platforms//runtime_es:posix",
    ],
    parents = [":x86_64-linux"],
)

platform(
    name = "x86_64-linux-gcc_8.3.0-posix",
    constraint_values = [
        "@score_bazel_platforms//version:gcc_8.3.0",
        "@score_bazel_platforms//runtime_es:posix",
    ],
    parents = [":x86_64-linux"],
)

platform(
    name = "x86_64-linux-glibc_2.39-autosd",
    constraint_values = [
        "@score_bazel_platforms//version:sdp_10.0",
        "@score_bazel_platforms//runtime_es:autosd",
    ],
    parents = [":x86_64-linux"],
)

platform(
    name = "aarch64-linux-glibc_2.39-autosd",
    constraint_values = [
        "@score_bazel_platforms//version:sdp_10.0",
        "@score_bazel_platforms//runtime_es:autosd",
    ],
    parents = [":aarch64-linux"],
)

platform(
    name = "aarch64-linux-sdk_0.1.0-ebclfsa",
    constraint_values = [
        "@score_bazel_platforms//version:sdk_0.1.0",
        "@score_bazel_platforms//runtime_es:ebclfsa",
    ],
    parents = [":aarch64-linux"],
)


# --------------------------------------------------------------------------------
# List of aliases (deprecated targets) to keep compatibility
# --------------------------------------------------------------------------------
alias(
    name = "arm64-linux",
    actual = ":aarch64-linux",
    deprecation = "This target is deprecated. Please use target `@score_bazel_platforms//:aarch64-linux`",
)

alias(
    name = "arm64-qnx",
    actual = ":aarch64-qnx",
    deprecation = "This target is deprecated. Please use target `@score_bazel_platforms//:aarch64-qnx`",
)

alias(
    name = "arm64-qnx8_0",
    actual = ":aarch64-qnx-sdp_8.0.0-posix",
    deprecation = "This target is deprecated. Please use target `@score_bazel_platforms//:aarch64-qnx-sdp_8.0.0-posix`",
)

alias(
    name = "arm64-qnx7_1",
    actual = ":aarch64-qnx-sdp_7.1.0-posix",
    deprecation = "This target is deprecated. Please use target `@score_bazel_platforms//:aarch64-qnx-sdp_7.1.0-posix`",
)

alias(
    name = "x86_64-qnx8_0",
    actual = ":x86_64-qnx-sdp_8.0.0-posix",
    deprecation = "This target is deprecated. Please use target `@score_bazel_platforms//:x86_64-qnx-sdp_8.0.0-posix`",
)

alias(
    name = "x86_64-qnx7_1",
    actual = ":x86_64-qnx-sdp_7.1.0-posix",
    deprecation = "This target is deprecated. Please use target `@score_bazel_platforms//:x86_64-qnx-sdp_7.1.0-posix`",
)

alias(
    name = "qnx8_0",
    actual = "@score_bazel_platforms//version:qnx_8.0.0",
    deprecation = "This target is deprecated. Please use target `@score_bazel_platforms//version:qnx_8.0.0`",
)

alias(
    name = "qnx7_1",
    actual = "@score_bazel_platforms//version:qnx_7.1.0",
    deprecation = "This target is deprecated. Please use target `@score_bazel_platforms//version:qnx_7.1.0`",
)
