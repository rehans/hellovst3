# Copyright(c) 2018 René Hansen.

# Version 3.10.0 is minimum for FetchContent
cmake_minimum_required (VERSION 3.11.0)

# Set default CMAKE_BUILD_TYPE to "Debug"
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    message(STATUS "Setting build type to 'Debug' as none was specified.")
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the type of build." FORCE)
endif()

# Use FetchContent to clone vst3sdk including all submodules
include(FetchContent)
FetchContent_Declare(
    vst3sdk
    GIT_REPOSITORY https://www.github.com/steinbergmedia/vst3sdk.git
    GIT_TAG d9eda1918e5bed70c51399c51a610ce9733f774a
    SOURCE_DIR ${FETCHCONTENT_BASE_DIR}/vst3sdk
)

# Create add_submodule macro which can be called to fetch and initialise the submodule
macro (add_submodule SUBMODULE_NAME)
    FetchContent_GetProperties(${SUBMODULE_NAME})
    if(NOT ${SUBMODULE_NAME}_POPULATED)
        message(STATUS "Fetching module '${SUBMODULE_NAME}' (SOURCE_DIR: ${FETCHCONTENT_BASE_DIR}/${SUBMODULE_NAME})")
        FetchContent_Populate(${SUBMODULE_NAME})
        add_subdirectory(${${SUBMODULE_NAME}_SOURCE_DIR} ${${SUBMODULE_NAME}_BINARY_DIR})
    endif()
endmacro()

# Project
project(HelloVST3)

# Fetch submodule 'vst3sdk' and use an additional 'include_directories' entry
add_submodule(vst3sdk)
include_directories(${vst3sdk_SOURCE_DIR})

smtg_add_vst3plugin(HelloVST3 "${vst3sdk_SOURCE_DIR}" source/main.cpp)
