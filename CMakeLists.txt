# Set minimum version of CMake.
cmake_minimum_required(VERSION 3.10)

# Set project name and version
project(qhttpserver VERSION 0.1.0)

# Set environment variables
include(${PROJECT_SOURCE_DIR}/cmake/SetEnv.cmake)

# Library sources
add_subdirectory(src)
