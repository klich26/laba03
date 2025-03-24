lab03
REPORT

$git clone https://github.com/klich26/lab02 projects/lab03
Cloning into 'projects/lab03'...
remote: Enumerating objects: 24, done.
remote: Counting objects: 100% (24/24), done.
remote: Compressing objects: 100% (16/16), done.
remote: Total 24 (delta 4), reused 21 (delta 3), pack-reused 0 (from 0)
Receiving objects: 100% (24/24), 8.38 KiB | 4.19 MiB/s, done.
Resolving deltas: 100% (4/4), done.

$cd project/lab03

$git remote remove origin

$git remote add https://github.com/klich26/lab03.git projects/lab03

$g++ -std=c++11 -I ./include/ -c sources/print.cpp 

$ls print.o
print.o

$nm print.o | grep print
0000000000000000 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
000000000000002a T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E

$ar rvs print.a print.o
ar: creating print.a
a - print.o

$file print.a 
print.a: current ar archive

$g++ -std=c++11 -I ./include -c examples/example1.cpp 
$ls example1.o
example1.o

$g++ example1.o print.a -o example1

$./example1 &&echo
hello

$g++ -std=c++11 -I ./include -c examples/example2.cpp

$nm example2.o
 
0000000000000000 V DW.ref.__gxx_personality_v0
                 U __gxx_personality_v0
0000000000000000 T main
                 U __stack_chk_fail
                 U _Unwind_Resume
                 U _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEEC1EPKcSt13_Ios_Openmode
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEED1Ev
0000000000000000 W _ZNSt15__new_allocatorIcED1Ev
0000000000000000 W _ZNSt15__new_allocatorIcED2Ev
0000000000000000 n _ZNSt15__new_allocatorIcED5Ev
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEC1EPKcRKS3_
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEED1Ev
                 U _ZSt21ios_base_library_initv
0000000000000000 r _ZStL19piecewise_construct

$g++ example2.o print.a -o example2

$./example2

$cat log.txt && echo 
hello

$rm -rf example1.o example2.o print.o

$rm -rf print.a
$rm -rf example1 example2

$rm -rf log.txt

$nano CMakeLists.txt
cmake_minimum_required(VERSION 3.4)
project(print)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(print STATIC /klich26/workspace/source/print.cpp)
include_directories(/klich26/workspace/projects/lab03/include)

$cmake -H. -B_build

-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /klich26/workspace/projects/lab03/_build



$cmake --build _build
-- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /klich26/workspace/projects/lab03/_build
root@admin11-NMH-WDX9:/klich26/workspace/projects/lab03# cmake --build _build/
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print

$nano CMakeLists.txt

add_executable(example1 /klich26/workspace/projects/lab03/examples/example1.cpp)
add_executable(example1 /klich26/workspace/projects/lab03/examples/example2.cpp)

target_link_libraries(example2 print)
target_link_libraries(example1 print)

$cmake --build _build 
- Configuring done (0.0s)
-- Generating done (0.0s)
-- Build files have been written to: /klich26/workspace/projects/lab03/_build
[ 33%] Built target print
[ 50%] Building CXX object CMakeFiles/example1.dir/examples/example1.cpp.o
[ 66%] Linking CXX executable example1
[ 66%] Built target example1
[ 83%] Building CXX object CMakeFiles/example2.dir/examples/example2.cpp.o
[100%] Linking CXX executable example2
[100%] Built target example2
$cmake --build _build --target print

[100%] Built target print

$cmake --build _build --target example1
[ 50%] Built target print
[100%] Built target example1

$cmake --build _build --target example2
[ 50%] Built target print
[100%] Built target example2

$ls -la _build/libprint.a 
-rw-r--r-- 1 root root 2238 Mar 22 22:55 _build/libprint.a

$_build/example1 && echo
hello

$_build/example2

$cat log.txt && echo
hello

$rm -rf log.txt

$git clone https://github.com/tp-labs/lab03 tmp
Cloning into 'tmp'...
remote: Enumerating objects: 91, done.
remote: Counting objects: 100% (30/30), done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 91 (delta 23), reused 21 (delta 21), pack-reused 61 (from 1)
Receiving objects: 100% (91/91), 1.02 MiB | 3.42 MiB/s, done.
Resolving deltas: 100% (41/41), done.

$mv -f tmp/CMakeLists.txt .

$rm -rf tmp

$cat CMakeLists.txt 
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_EXAMPLES "Build examples" OFF)

project(print)

add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)

target_include_directories(print PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

if(BUILD_EXAMPLES)
  file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
  foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
    get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
    add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE})
    target_link_libraries(${EXAMPLE_NAME} print)
    install(TARGETS ${EXAMPLE_NAME}
      RUNTIME DESTINATION bin
    )
  endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif()

install(TARGETS print
    EXPORT print-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
install(EXPORT print-config DESTINATION cmake)

$cmake -H. -B_build -DCMAKE_NSTALL_PREFIX=_install
CMake Deprecation Warning at CMakeLists.txt:1 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Configuring done (0.0s)
-- Generating done (0.0s)
CMake Warning:
  Manually-specified variables were not used by the project:

    CMAKE_NSTALL_PREFIX


-- Build files have been written to: /klich26/workspace/projects/lab03/_build


$cmake --build _build/ --target install
[100%] Built target print
Install the project...
-- Install configuration: ""
-- Installing: /usr/local/lib/libprint.a
-- Up-to-date: /usr/local/include
-- Installing: /usr/local/include/print.hpp
-- Installing: /usr/local/cmake/print-config.cmake
-- Installing: /usr/local/cmake/print-config-noconfig.cmake



$tree _install

/klich26/workspace/projects/lab03/_install/
├── cmake
│   ├── print-config.cmake
│   └── print-config-noconfig.cmake
├── include
│   └── print.hpp
└── lib
    └── libprint.a

4 directories, 4 files

$git add CMakeLists.txt

$git commit -m "added CMakeLists.txt"
$git push origin main
