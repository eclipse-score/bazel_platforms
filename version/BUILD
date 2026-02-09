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
# OS version constraint settings definitions and their values
# --------------------------------------------------------------------------------
constraint_setting(name = "os_version")

constraint_value(
    name = "qnx_7.1.0",
    constraint_setting = ":os_version",
)

constraint_value(
    name = "qnx_8.0.0",
    constraint_setting = ":os_version",
)

# --------------------------------------------------------------------------------
# SDP version constraint settings definitions and their values (alias for OS ver.)
# --------------------------------------------------------------------------------
alias(
    name = "sdp_7.1.0",
    actual = "qnx_8.0.0",
)

alias(
    name = "sdp_8.0.0",
    actual = "qnx_8.0.0",
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

constraint_value(
    name = "gcc_autosd-10.0",
    constraint_setting = ":gcc_version",
)

