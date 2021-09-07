###
#  @file CMakeLists.txt
#  @license
#    Copyright (c) 2013-2017
#    National Taiwan University
#    National Tsing-Hua University
#
#    This file is part of Uni10, the Universal Tensor Network Library.
#
#    Uni10 is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Lesser General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    Uni10 is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Lesser General Public License for more details.
#
#    You should have received a copy of the GNU Lesser General Public License
#    along with Uni10.  If not, see <http://www.gnu.org/licenses/>.
#  @endlicense
#  @brief Specification file for CMake
#  @author Ying-Jer Kao
#  @date 2014-05-06
#  @since 0.9.0
###


######################################################################
### LIST OF FILES
######################################################################
enable_testing()
include_directories(${gtest_SOURCE_DIR}/include)
include_directories(${gmock_SOURCE_DIR}/include)
################################
# Unit Tests
################################

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -std=c++11")
set(test_sources
  CytnxTest.cpp
  testBond.h
)
# Add test cpp file
add_executable( runCytnxTests ${test_sources})
# Link test executable against gtest & gtest_main
target_link_libraries(runCytnxTests pthread gtest gtest_main cytnx)
#if(CMAKE_BUILD_TYPE MATCHES Debug AND NOT DISABLE_CODE_COVERAGE)
#  include(CodeCoverage)
#  set(COVERAGE_EXCLUDES '/usr/.*' 'gtest-1.7.0/.*')
#  setup_target_for_coverage(
#    NAME uni10_coverage
#    EXECUTABLE runUni10Tests
#    DEPENDENCIES runUni10Tests)
#  setup_target_for_coverage_cobertura(
#    NAME uni10_cobertura
#    EXECUTABLE runUni10Tests)
#endif()
file(COPY "${CMAKE_CURRENT_SOURCE_DIR}/testNet.net"
      DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
file(COPY "${CMAKE_CURRENT_SOURCE_DIR}/testNet.net"
      DESTINATION ${CMAKE_BINARY_DIR})
add_test( runUni10Tests runUni10Tests --gtest_output=xml)

######################################################################
### BUILD EXAMPLES
######################################################################
install(TARGETS runCytnxTests DESTINATION uni10_api_test/ COMPONENT test)
