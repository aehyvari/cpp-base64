cmake_minimum_required(VERSION 3.3)

set(CPPBASE64_VERSION_MAJOR 0)
set(CPPBASE64_VERSION_MINOR 0)
set(CPPBASE64_VERSION_PATCH 0)
set(CPPBASE64_VERSION ${CPPBASE64_VERSION_MAJOR}.${CPPBASE64_VERSION_MINOR}.${CPPBASE64_VERSION_PATCH})
project(cppbase64 VERSION ${CPPBASE64_VERSION} LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)

set(CMAKE_SOURCE_DIR ".")

#Set the default build type if this is the first time cmake is run and nothing has been set
if (NOT EXISTS ${CMAKE_BINARY_DIR}/CMakeCache.txt)
  if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel." FORCE)
  endif()
endif()

add_library(base64 SHARED ${CMAKE_SOURCE_DIR}/base64.cpp)

set_target_properties(base64 PROPERTIES
    VERSION ${CPPBASE64_VERSION}
    OUTPUT_NAME base64
    PUBLIC_HEADER base64.h
)

include(GNUInstallDirs)

install(TARGETS base64
     PUBLIC_HEADER
         DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/base64
)
