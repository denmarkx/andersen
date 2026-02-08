cmake_minimum_required(VERSION 2.8.8)
project(andersen)

find_package(LLVM REQUIRED CONFIG)

message(STATUS "Found LLVM ${LLVM_PACKAGE_VERSION}")
message(STATUS "Using LLVMConfig.cmake in: ${LLVM_DIR}")

# Set your project compile flags.
# E.g. if using the C++ header files
# you will need to enable C++17 support
# for your compiler.
# Check for C++17 support and set the compilation flag
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++17" COMPILER_SUPPORTS_CXX14)
if(COMPILER_SUPPORTS_CXX14)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -fno-rtti -fPIC -Wall")
else()
	message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++17 support. Please use a different C++ compiler.")
endif()

option(BUILD_TESTS "build all unit tests" ON)

include_directories(${LLVM_INCLUDE_DIRS})
add_definitions(${LLVM_DEFINITIONS})

add_subdirectory (lib)
if (BUILD_TESTS)
	add_subdirectory (unittest)
endif()

enable_testing ()
add_test (AndersTest ${PROJECT_BINARY_DIR}/unittest/AndersTest)