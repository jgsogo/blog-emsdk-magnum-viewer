cmake_minimum_required(VERSION 3.20)
project(ExampleMagnum)

set(CMAKE_CXX_STANDARD 11)

# New Conan versions don't require these lines
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup(TARGETS)

set(CONAN_STL "${CMAKE_CURRENT_LIST_DIR}/resources/conan.stl")
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/configure.h.in
               ${CMAKE_CURRENT_BINARY_DIR}/configure.h)
include_directories(${CMAKE_CURRENT_BINARY_DIR})

find_package(Corrade REQUIRED Main)
find_package(Magnum REQUIRED GL)

add_subdirectory(viewer)
