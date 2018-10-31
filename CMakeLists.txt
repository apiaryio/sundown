cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

project(Sundown VERSION 1.0 LANGUAGES C)

set(Sundown_SOURCES
    src/autolink.c
    src/buffer.c
    src/markdown.c
    src/src_map.c
    src/stack.c
    )

add_library(sundown ${Sundown_SOURCES})
add_library(sundown-pic STATIC ${Sundown_SOURCES})
add_library(sundown-static STATIC ${Sundown_SOURCES})
set_property(TARGET sundown-pic PROPERTY POSITION_INDEPENDENT_CODE 1)

target_include_directories(sundown PUBLIC
    $<BUILD_INTERFACE:${Sundown_BINARY_DIR}/src>
    $<BUILD_INTERFACE:${Sundown_SOURCE_DIR}/src>
    $<INSTALL_INTERFACE:src>
    )

target_include_directories(sundown-pic PUBLIC
    $<BUILD_INTERFACE:${Sundown_BINARY_DIR}/src>
    $<BUILD_INTERFACE:${Sundown_SOURCE_DIR}/src>
    $<INSTALL_INTERFACE:src>
    )

target_include_directories(sundown-static PUBLIC
    $<BUILD_INTERFACE:${Sundown_BINARY_DIR}/src>
    $<BUILD_INTERFACE:${Sundown_SOURCE_DIR}/src>
    $<INSTALL_INTERFACE:src>
    )

install(TARGETS sundown sundown-static sundown-pic EXPORT sundown-targets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
    INCLUDES DESTINATION src
    )

install(EXPORT sundown-targets
    FILE SundownTargets.cmake
    NAMESPACE Apiary::
    DESTINATION lib/cmake/apiary
    )

include(CMakePackageConfigHelpers)
write_basic_package_version_file("SundownConfigVersion.cmake"
    VERSION ${Sundown_VERSION}
    COMPATIBILITY SameMajorVersion
    )
install(FILES
    "SundownConfig.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/SundownConfigVersion.cmake"
    DESTINATION lib/cmake/apiary
    )

add_library(Apiary::sundown ALIAS sundown)
add_library(Apiary::sundown-static ALIAS sundown-static)
add_library(Apiary::sundown-pic ALIAS sundown-pic)
