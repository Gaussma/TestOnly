cmake_minimum_required(VERSION 3.1 FATAL_ERROR)

project(msvc_boost_test CXX)
set(CMAKE_CXX_STANDARD 11)

if(NOT CMAKE_BUILD_TYPE)
   set(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the build type" FORCE)
endif(NOT CMAKE_BUILD_TYPE)

enable_testing()
add_custom_target(check COMMAND ${CMAKE_CTEST_COMMAND} --output-on-failure -C ${CMAKE_BUILD_TYPE})

find_package(Boost REQUIRED COMPONENTS unit_test_framework)
include_directories(${Boost_INCLUDE_DIRS})

function(UNIT_TEST)
  # Parse arguments
  cmake_parse_arguments(UT "" "TARGET" "SOURCE" ${ARGN})

  # Add target for test and link with libraries
  add_executable(${UT_TARGET} ${UT_SOURCE})
  target_link_libraries(${UT_TARGET} ${LIBRARIES} ${Boost_UNIT_TEST_FRAMEWORK_LIBRARY})

  add_test(NAME ${UT_TARGET} COMMAND ${UT_TARGET})
  add_dependencies(check ${UT_TARGET})
endfunction(UNIT_TEST)

unit_test(TARGET target_boost_test SOURCE target_boost_test.cpp)