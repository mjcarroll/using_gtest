cmake_minimum_required(VERSION 3.10.2)

project(using_gtest LANGUAGES CXX VERSION 0.0.1)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(ament_cmake QUIET)

# Thanks to https://github.com/facontidavide/PlotJuggler/blob/main/CMakeLists.txt#L66
if(ament_cmake_FOUND)
  set(COMPILING_WITH_AMENT 1)
  find_package(ament_cmake REQUIRED)

  message(STATUS "--------------------------------------------------")
  message(STATUS "Project is being built using AMENT.")
  message(STATUS "--------------------------------------------------")

  add_definitions(-DCOMPILED_WITH_AMENT)
endif()


if(BUILD_TESTING)
  if(COMPILING_WITH_AMENT)
    find_package(gtest_vendor REQUIRED)
    find_package(ament_cmake_gtest REQUIRED)

    ament_find_gtest()
    add_library(GTest::gtest ALIAS gtest)
    add_library(GTest::gtest_main ALIAS gtest_main)

  else()
    find_package(GTest REQUIRED)
  endif()

  add_executable(using_gtest using_gtest.cc)
  target_link_libraries(using_gtest PUBLIC GTest::gtest GTest::gtest_main)
endif()
