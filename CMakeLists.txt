# Copyright (c) 2015 Samsung Electronics Co., Ltd. All rights reserved.
# Use of this source code is governed by a BSD-style license that can be
# found in the LICENSE file.

# Build script for RPM packaging

CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

PROJECT("node-xwalk")

INCLUDE(FindPkgConfig)

IF(NOT CMAKE_BUILD_TYPE)
    SET(CMAKE_BUILD_TYPE "Release")
ENDIF(NOT CMAKE_BUILD_TYPE)

SET(CMAKE_C_FLAGS_PROFILING    "-O2")
SET(CMAKE_CXX_FLAGS_PROFILING  "-O2 -std=c++0x")
SET(CMAKE_C_FLAGS_DEBUG        "-O0 -g")
SET(CMAKE_CXX_FLAGS_DEBUG      "-O0 -std=c++0x -g")
SET(CMAKE_C_FLAGS_RELEASE      "-O2 -g")
SET(CMAKE_CXX_FLAGS_RELEASE    "-O2 -std=c++0x -g -fvisibility-inlines-hidden")
SET(CMAKE_CXX_FLAGS_CCOV       "-O0 -std=c++0x -g --coverage")

ADD_DEFINITIONS("-fPIC")
ADD_DEFINITIONS("-Wall")                # Generate all warnings
ADD_DEFINITIONS("-Wextra")              # Generate even more extra warnings
ADD_DEFINITIONS("-Wno-variadic-macros") # Inhibit variadic macros warnings (needed for ORM)
ADD_DEFINITIONS("-Wno-deprecated")      # No warnings about deprecated features
ADD_DEFINITIONS("-std=c++0x")           # accept C++11x standard

SET(GLOBAL_NODE_MODULE_PATH /usr/local/lib/node_modules)

# Copy Project
INSTALL(FILES
    ${PROJECT_SOURCE_DIR}/package.json
    DESTINATION ${GLOBAL_NODE_MODULE_PATH}/${PROJECT_NAME}
)

INSTALL(FILES
    ${PROJECT_SOURCE_DIR}/lib/loader.js
    DESTINATION ${GLOBAL_NODE_MODULE_PATH}/${PROJECT_NAME}/lib
)

# Native Binding Module
SET(TARGET_NATIVE "native")

PKG_CHECK_MODULES(TARGET_NATIVE_DEPS REQUIRED
    nodejs
)

INCLUDE_DIRECTORIES (
  ${PROJECT_SOURCE_DIR}/src
  ${TARGET_NATIVE_DEPS_INCLUDE_DIRS}
)
ADD_LIBRARY(${TARGET_NATIVE} MODULE
  ${PROJECT_SOURCE_DIR}/src/native_binding.cc
  ${PROJECT_SOURCE_DIR}/src/extension.cc
  ${PROJECT_SOURCE_DIR}/src/extension_adapter.cc
)
SET_TARGET_PROPERTIES(${TARGET_NATIVE} PROPERTIES PREFIX "" SUFFIX ".node")
INSTALL(TARGETS ${TARGET_NATIVE} DESTINATION
        ${GLOBAL_NODE_MODULE_PATH}/${PROJECT_NAME}/build/Release)

