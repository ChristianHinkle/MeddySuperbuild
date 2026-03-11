# Copyright (c) 2023-2024 Christian Hinkle, Brian Hinkle.

cmake_minimum_required(VERSION 4.0)

project(MeddySDKSuperbuildStandalone
  LANGUAGES CXX
  )

include(ExternalProject)

set(SHARED_CMAKE_ARGS
  "-DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE} -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX} -DCMAKE_PREFIX_PATH=${CMAKE_PREFIX_PATH}"
  )

ExternalProject_Add(CppUtils
  SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/CppUtils"
  CMAKE_ARGS ${SHARED_CMAKE_ARGS}
  )

ExternalProject_Add(Boost
  SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/boost"
  CMAKE_ARGS ${SHARED_CMAKE_ARGS}
  )

ExternalProject_Add(MeddySDK
  DEPENDS CppUtils
  DEPENDS Boost
  SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/MeddySDK"
  CMAKE_ARGS ${SHARED_CMAKE_ARGS}
  )

ExternalProject_Add(MeddyCLI
  DEPENDS MeddySDK
  SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/MeddyCLI"
  CMAKE_ARGS ${SHARED_CMAKE_ARGS}
  )

# Unset this variable, as it is no longer needed.
unset(SHARED_CMAKE_ARGS)
