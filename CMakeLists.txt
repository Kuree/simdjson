cmake_minimum_required(VERSION 3.9) # CMP0069 NEW

if (NOT CMAKE_BUILD_TYPE)
  message(STATUS "No build type selected, default to Release")
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
endif()

project(simdjson
  DESCRIPTION "Parsing gigabytes of JSON per second"
  LANGUAGES CXX
)

# LTO seems to create all sorts of fun problems. Let us
# disable temporarily.
#include(CheckIPOSupported)
#check_ipo_supported(RESULT ltoresult)
#if(ltoresult)
#  set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)
#endif()

# usage: cmake -DSIMDJSON_DISABLE_AVX=on ..
option(SIMDJSON_DISABLE_AVX "Forcefully disable AVX even if hardware supports it" OFF)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_MACOSX_RPATH OFF)
set(CMAKE_THREAD_PREFER_PTHREAD ON)
set(THREADS_PREFER_PTHREAD_FLAG ON)


add_library(simdjson src/simdjson.cpp)
target_include_directories(simdjson PRIVATE include/simdjson)
