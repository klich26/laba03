cmake_minimum_required(VERSION 3.4)
project(hello_world)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED on)
add_subdirectory(/klich26/workspace/projects/lab03/formatter_ex formatter_ex_lib_dir)

add_executable(hello_world /klich26/workspace/projects/lab03/hello_world_application/hello_world.cpp)

target_include_directories(hello_world PUBLIC /klich26/workspace/projects/lab03/formatter_ex_lib)

target_link_libraries(hello_world formatter_ex_lib)

