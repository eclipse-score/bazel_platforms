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

""" This file holds defintions of custom constraints and their vaules related to version
"""

package(default_visibility = ["//visibility:public"])

# --------------------------------------------------------------------------------
# SDP version constraint settings definitions and their values
# --------------------------------------------------------------------------------
constraint_setting(name = "sdp_version")

constraint_value(
    name = "sdp_7.1.0",
    constraint_setting = ":sdp_version",
)

constraint_value(
    name = "sdp_8.0.0",
    constraint_setting = ":sdp_version",
)

constraint_value(
    name = "sdp_10.0",
    constraint_setting = ":sdp_version",
)

# --------------------------------------------------------------------------------
# SDK version constraint settings definitions and their values
# --------------------------------------------------------------------------------
constraint_setting(name = "sdk_version")

constraint_value(
    name = "sdk_0.1.0",
    constraint_setting = ":sdk_version",
)

# --------------------------------------------------------------------------------
# SDK version constraint settings definitions and their values
# --------------------------------------------------------------------------------
constraint_setting(name = "glibc_version")

constraint_value(
    name = "glibc_2.39",
    constraint_setting = ":sdk_version",
)

# --------------------------------------------------------------------------------
# GCC version constraint settings definitions and their values
# --------------------------------------------------------------------------------
constraint_setting(name = "gcc_version")

constraint_value(
    name = "gcc_12.2.0",
    constraint_setting = ":gcc_version",
)

constraint_value(
    name = "gcc_8.3.0",
    constraint_setting = ":gcc_version",
)

# --------------------------------------------------------------------------------
# Deprecated values
# --------------------------------------------------------------------------------
alias(
    name = "qnx_8.0.0",
    actual = ":sdk_8.0.0",
    deprecation = "This target is deprecated. Please use target `@score_bazel_platforms//version:sdk_8.0.0`",
)

alias(
    name = "qnx_7.1.0",
    actual = ":sdk_7.1.0",
    deprecation = "This target is deprecated. Please use target `@score_bazel_platforms//version:sdk_7.1.0`",
)
