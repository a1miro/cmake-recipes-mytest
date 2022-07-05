cmake_minimum_required(VERSION 3.0.0)
project(mytest VERSION 0.1.0)

include(CTest)
include(GoogleTest)

enable_testing()

list(APPEND CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")


#TODO: Find out how to use find_package instead of find_library
#find_package(GTest REQUIRED)

find_library(LIBGTEST NAMES gtest)
message(DEBUG "LIBGTEST=${LIBGTEST}")
if(LIBGTEST MATCHES "^.*NOT.*FOUND.*$")
  set(LIBGTEST "gtest")
  message(DEBUG "google test is not installed, fetching sources and building it")
  set(CMAKE_POLICY_DEFAULT_CMP0048 NEW)
  include(googletest)
endif()

add_executable(mytest mytest.cpp)

target_link_libraries(mytest gtest gtest_main)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

gtest_discover_tests(mytest)