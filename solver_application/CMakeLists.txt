cmake_minimum_required(VERSION 3.4)

project(solver)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(/klich26/workspace/projects/lab03/formatter_ex formatter_ex_lib_dir)
add_subdirectory(/klich26/workspace/projects/lab03/solver_lib/ solver_lib_dir)

add_executable(solver /klich26/workspace/projects/lab03/solver_application/equation.cpp)

target_include_directories(formatter_ex_lib PUBLIC /klich26/workspace/projects/lab03/formatter_ex
/klich26/workspace/projects/lab03/solver_lib
)

target_link_libraries(solver formatter_ex_lib solver_lib)
