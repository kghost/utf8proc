cmake_minimum_required (VERSION 2.8)

include (utils.cmake)

disallow_intree_builds()

project (utf8proc C)

# This is the ABI version number, which may differ from the
# API version number (defined in utf8proc.h).
# Be sure to also update these in Makefile and MANIFEST!
set(SO_MAJOR 2)
set(SO_MINOR 1)
set(SO_PATCH 0)

add_definitions (
  -DUTF8PROC_EXPORTS
)

if (NOT MSVC)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2 -std=c99 -pedantic -Wall")
endif ()

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

add_library (utf8proc SHARED
  utf8proc.c
  utf8proc.h
)

target_include_directories(utf8proc
  PUBLIC
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>"
)

set_target_properties (utf8proc PROPERTIES
  POSITION_INDEPENDENT_CODE ON
  VERSION "${SO_MAJOR}.${SO_MINOR}.${SO_PATCH}"
  SOVERSION ${SO_MAJOR}
)

configure_file(
    "${PROJECT_SOURCE_DIR}/cmake/utf8proc-config.cmake.in"
    "${CMAKE_BINARY_DIR}/utf8proc-config.cmake"
    @ONLY
)

install(FILES
    "${CMAKE_BINARY_DIR}/utf8proc-config.cmake"
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/utf8proc"
    COMPONENT Devel
)

export(TARGETS utf8proc
  FILE "${CMAKE_BINARY_DIR}/utf8proc-targets.cmake"
  EXPORT_LINK_INTERFACE_LIBRARIES
)
