cmake_minimum_required(VERSION 3.4 FATAL_ERROR)

project(sundown-apiary VERSION 1.0 LANGUAGES C)

add_library(sundown-apiary
    src/autolink.c
    src/buffer.c
    src/markdown.c
    src/src_map.c
    src/stack.c
    )

target_include_directories(sundown-apiary PUBLIC
    $<BUILD_INTERFACE:${sundown-apiary_BINARY_DIR}/src>
    $<BUILD_INTERFACE:${sundown-apiary_SOURCE_DIR}/src>
    $<INSTALL_INTERFACE:src>
    )

install(TARGETS sundown-apiary EXPORT sundown-apiary-targets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
    INCLUDES DESTINATION src
    )

install(EXPORT sundown-apiary-targets
    FILE sundown-apiary-targets.cmake
    NAMESPACE sundown-apiary::
    DESTINATION lib/cmake/sundown-apiary
    )

include(CMakePackageConfigHelpers)
write_basic_package_version_file("sundown-apiary-config-version.cmake"
    VERSION ${sundown-apiary_VERSION}
    COMPATIBILITY SameMajorVersion
    )
install(FILES
    "sundown-apiary-config.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/sundown-apiary-config-version.cmake"
    DESTINATION lib/cmake/sundown-apiary
    )

add_library(sundown-apiary::sundown-apiary ALIAS sundown-apiary)
