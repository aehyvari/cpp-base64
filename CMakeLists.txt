cmake_minimum_required(VERSION 3.3)

set(BASE64_VERSION_MAJOR 0)
set(BASE64_VERSION_MINOR 0)
set(BASE64_VERSION_PATCH 0)
set(BASE64_VERSION ${BASE64_VERSION_MAJOR}.${BASE64_VERSION_MINOR}.${BASE64_VERSION_PATCH})
project(base64 VERSION ${BASE64_VERSION} LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)

set(CMAKE_SOURCE_DIR ".")

#Set the default build type if this is the first time cmake is run and nothing has been set
if (NOT EXISTS ${CMAKE_BINARY_DIR}/CMakeCache.txt)
  if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel." FORCE)
  endif()
endif()

include(GNUInstallDirs)

add_library(base64 SHARED ${CMAKE_SOURCE_DIR}/base64.cpp)

set_target_properties(base64 PROPERTIES
    VERSION ${BASE64_VERSION}
    OUTPUT_NAME base64
    PUBLIC_HEADER base64.h
)

target_include_directories(base64
    PUBLIC
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>"
    "$<INSTALL_INTERFACE:$<INSTALL_PREFIX>/include/Base64Targets>"
)

install(TARGETS base64
        EXPORT Base64Targets
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
        PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

install(FILES base64.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

include(CMakePackageConfigHelpers)

configure_package_config_file(${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in
        "${CMAKE_CURRENT_BINARY_DIR}/Base64Config.cmake"
        INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/Base64
)

install(FILES
        "${CMAKE_CURRENT_BINARY_DIR}/Base64Config.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/Base64ConfigVersion.cmake"
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/Base64
)

set_property(TARGET base64 PROPERTY VERSION ${BASE64_VERSION})
set_property(TARGET base64 PROPERTY SOVERSION 0)
set_property(TARGET base64 PROPERTY INTERFACE_Base64_MAJOR_VERSION 0)
set_property(TARGET base64 APPEND PROPERTY COMPATIBLE_INTERFACE_STRING Base64_MAJOR_VERSION)

write_basic_package_version_file(
        "${CMAKE_CURRENT_BINARY_DIR}/Base64ConfigVersion.cmake"
        VERSION "${BASE64_VERSION}"
        COMPATIBILITY AnyNewerVersion
)

export(EXPORT Base64Targets
        FILE "${CMAKE_CURRENT_BINARY_DIR}/cmake/Base64Targets.cmake"
        NAMESPACE Base64::
)