# Copyright(c) 2016 René Hansen.

cmake_minimum_required (VERSION 3.11.0)

# Fetch content
include(FetchContent)
FetchContent_Declare(
    vst3sdk
    GIT_REPOSITORY https://www.github.com/steinbergmedia/vst3sdk.git
    GIT_TAG d9eda1918e5bed70c51399c51a610ce9733f774a
    SOURCE_DIR ${FETCHCONTENT_BASE_DIR}/vst3sdk
)


macro (add_submodule SUBMODULE_NAME)
    FetchContent_GetProperties(${SUBMODULE_NAME})
    if(NOT ${SUBMODULE_NAME}_POPULATED)
        message(STATUS "Fetching module ${SUBMODULE_NAME}")
        FetchContent_Populate(${SUBMODULE_NAME})
        add_subdirectory(${${SUBMODULE_NAME}_SOURCE_DIR} ${${SUBMODULE_NAME}_BINARY_DIR})
    endif()
endmacro()

# Project
project(HelloVST3)

add_submodule(vst3sdk)

include_directories(${FETCHCONTENT_BASE_DIR}/vst3sdk)
add_executable(HelloVST3 source/main.cpp)
