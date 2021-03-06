project(apriltag C CXX)

cmake_minimum_required ( VERSION 2.8 )

if (POLICY CMP0042)
cmake_policy(SET CMP0042 NEW)
endif(POLICY CMP0042)

find_package (Threads REQUIRED)

if (${CMAKE_SYSTEM_PROCESSOR} MATCHES "^armv7*")
  # for Raspberry Pi 3
  set(arch_settings "-march=armv7-a")
else()
  set(arch_settings "-march=native")
endif()
set(EXTRA_FLAGS "-Wall -Wsign-compare -g ${arch_settings} ${CMAKE_THREAD_LIBS_INIT}")

set(EXTRA_C_FLAGS "${EXTRA_FLAGS} -std=gnu99")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${EXTRA_FLAGS}")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${EXTRA_C_FLAGS}")

set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/lib)

if(APPLE)
  SET(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)
endif(APPLE)

add_subdirectory(core)
include_directories(core core/common core/contrib)

include(FindPkgConfig)
pkg_search_module(OPENCV opencv>=4)

if(OPENCV_FOUND)
  include_directories(${OPENCV_INCLUDE_DIRS})
  add_subdirectory(opencv)
endif(OPENCV_FOUND)

