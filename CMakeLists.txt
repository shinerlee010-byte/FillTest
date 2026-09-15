cmake_minimum_required(VERSION 3.16)
project(NurbsFiller
    VERSION 0.1.0
    DESCRIPTION "Constrained NURBS surface fitting library"
    LANGUAGES CXX
)

# ----------------------------------------------------------------------------
# Global compile options
# ----------------------------------------------------------------------------
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)   # 便于 VSCode clangd / IntelliSense

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "" FORCE)
endif()

if(MSVC)
    add_compile_options(/W4 /permissive- /Zc:__cplusplus)
    add_compile_definitions(_CRT_SECURE_NO_WARNINGS NOMINMAX)
else()
    add_compile_options(-Wall -Wextra -Wpedantic -Wshadow -Wconversion)
endif()

# ----------------------------------------------------------------------------
# Dependencies (Eigen header-only, GoogleTest for tests)
# ----------------------------------------------------------------------------
include(FetchContent)

set(FETCHCONTENT_QUIET OFF)

# Eigen: header-only, very stable
FetchContent_Declare(
    eigen
    GIT_REPOSITORY https://gitlab.com/libeigen/eigen.git
    GIT_TAG        3.4.0
    GIT_SHALLOW    TRUE
)

# GoogleTest only when tests are enabled
option(NURBSFILLER_BUILD_TESTS "Build unit tests" ON)

if(NURBSFILLER_BUILD_TESTS)
    FetchContent_Declare(
        googletest
        GIT_REPOSITORY https://github.com/google/googletest.git
        GIT_TAG        v1.14.0
        GIT_SHALLOW    TRUE
    )
    set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
endif()

FetchContent_MakeAvailable(eigen)
if(NURBSFILLER_BUILD_TESTS)
    FetchContent_MakeAvailable(googletest)
endif()

# ----------------------------------------------------------------------------
# Library target
# ----------------------------------------------------------------------------
add_subdirectory(src)

# ----------------------------------------------------------------------------
# CLI application
# ----------------------------------------------------------------------------
add_subdirectory(app)

# ----------------------------------------------------------------------------
# Tests
# ----------------------------------------------------------------------------
if(NURBSFILLER_BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()

# ----------------------------------------------------------------------------
# Status / summary
# ----------------------------------------------------------------------------
message(STATUS "===========================================")
message(STATUS " NurbsFiller ${PROJECT_VERSION}")
message(STATUS "   CXX: ${CMAKE_CXX_COMPILER_ID} ${CMAKE_CXX_COMPILER_VERSION}")
message(STATUS "   Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "   Tests: ${NURBSFILLER_BUILD_TESTS}")
message(STATUS "===========================================")