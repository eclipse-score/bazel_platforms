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

platform(
    name = "arm64-linux",
    constraint_values = [
        "@platforms//cpu:arm64",
        "@platforms//os:linux",
    ],
)

platform(
    name = "arm64-qnx",
    constraint_values = [
        "@platforms//cpu:arm64",
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

# Warning: Those platforms will be removed for release 0.5 final latest!
# Below we define a platform to align with toolchain_qnx definitions of toolchains.
# This all going to be be removed once we migrate to use only the bazel https://github.com/eclipse-score/bazel_cpp_toolchains/
# and we also migrate toolchain_rust to follow the same pattern as in bazel_cpp_toolchains.
platform(
    name = "arm64-qnx8_0",
    parents = [":arm64-qnx"],
    constraint_values = [
        ":qnx8_0",
    ],
)

platform(
    name = "arm64-qnx7_1",
    parents = [":arm64-qnx"],
    constraint_values = [
        ":qnx7_1",
    ],
)

platform(
    name = "x86_64-qnx8_0",
    parents = [":x86_64-qnx"],
    constraint_values = [
        ":qnx8_0",
    ],
)

platform(
    name = "x86_64-qnx7_1",
    parents = [":x86_64-qnx"],
    constraint_values = [
        ":qnx7_1",
    ],
)

constraint_setting(
    name = "qnx_version",
    visibility = ["//visibility:public"],
)

constraint_value(
    name = "qnx7_1",
    constraint_setting = ":qnx_version",
    visibility = ["//visibility:public"],
)

constraint_value(
    name = "qnx8_0",
    constraint_setting = ":qnx_version",
    visibility = ["//visibility:public"],
)