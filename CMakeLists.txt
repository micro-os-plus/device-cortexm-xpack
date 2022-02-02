#
# This file is part of the µOS++ distribution.
#   (https://github.com/micro-os-plus)
# Copyright (c) 2021 Liviu Ionescu
#
# This Source Code Form is subject to the terms of the MIT License.
# If a copy of the license was not distributed with this file, it can
# be obtained from https://opensource.org/licenses/MIT/.
#
# -----------------------------------------------------------------------------

# This file is intended to be consumed by applications with:
#
# `add_subdirectory("xpacks/micro-os-plus-micro-test-plus")`
#
# The result is a library that can be added to the linker with:
#
# `target_link_libraries(your-target PUBLIC micro-os-plus::micro-test-plus)`

# -----------------------------------------------------------------------------
## Preamble ##

# https://cmake.org/cmake/help/v3.20/
cmake_minimum_required(VERSION 3.20)

if(COMMAND xpack_get_package_name_and_version)
  xpack_get_package_name_and_version("${CMAKE_CURRENT_SOURCE_DIR}/package.json")
  message(VERBOSE "Processing xPack ${PACKAGE_JSON_NAME}@${PACKAGE_JSON_VERSION}...")
endif()

# -----------------------------------------------------------------------------
## The project library definitions ##

# https://cmake.org/cmake/help/v3.20/command/add_library.html?highlight=interface#normal-libraries
# PRIVATE: build definitions, used internally
# INTERFACE: usage definitions, passed up to targets linking to it
# PUBLIC: both

add_library(micro-os-plus-device-cortexm-interface INTERFACE EXCLUDE_FROM_ALL)

# -----------------------------------------------------------------------------
# Target settings.

target_sources(
  micro-os-plus-device-cortexm-interface

  INTERFACE
    "src/diag/trace-itm.cpp"
    "src/diag/trace-segger-rtt.cpp"

    "src/startup/initialize-hardware-early.c"
    "src/startup/initialize-hardware.c"
    "src/startup/initialize-interrupts-stack.cpp"
    "src/startup/terminate.cpp"

    "src/exception-handlers.cpp"
  )

message(VERBOSE "+ src/diag/trace-itm.cpp")
message(VERBOSE "+ src/diag/trace-segger-rtt.cpp")

message(VERBOSE "+ src/startup/initialize-hardware-early.c")
message(VERBOSE "+ src/startup/initialize-hardware.c")
message(VERBOSE "+ src/startup/initialize-interrupts-stack.cpp")
message(VERBOSE "+ src/startup/terminate.cpp")

message(VERBOSE "+ src/exception-handlers.cpp")

target_include_directories(
  micro-os-plus-device-cortexm-interface

  INTERFACE
    "include"
)

message(VERBOSE "+ include")

target_compile_definitions(
  micro-os-plus-device-cortexm-interface

  INTERFACE
    # Preprocessor macro definitions
)

target_compile_options(
  micro-os-plus-device-cortexm-interface

  INTERFACE
    # Perhaps ignore some warnings.
)

target_link_options(
  micro-os-plus-device-cortexm-interface

  INTERFACE
    # Linker options
)

target_link_libraries(
  micro-os-plus-device-cortexm-interface

  INTERFACE
    # Dependencies
    micro-os-plus::semihosting
)

# -----------------------------------------------------------------------------
# Aliases.

add_library(micro-os-plus::device-cortexm ALIAS micro-os-plus-device-cortexm-interface)
message(VERBOSE "> micro-os-plus::device-cortexm -> micro-os-plus-device-cortexm-interface")

# -----------------------------------------------------------------------------
