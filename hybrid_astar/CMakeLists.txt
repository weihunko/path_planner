cmake_minimum_required(VERSION 2.8.3)
project(hybrid_astar)

## C++14
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++14" COMPILER_SUPPORTS_CXX14)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX14)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -Wall")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++14 support. Please use a different C++ compiler.")
endif()

## OPEN MOTION PLANNING LIBRARY
find_package(OMPL REQUIRED)
find_package(catkin REQUIRED COMPONENTS
    roscpp
    rospy
    std_msgs
    tf
    )

set(HEADERS
    include/algorithm.h
    include/node2d.h
    include/node3d.h
    include/collisiondetection.h
    include/planner.h
    include/path.h
    include/smoother.h
    include/vector2d.h
    include/visualize.h
    include/helper.h
    include/constants.h
    include/lookup.h
    include/gradient.h #Andrew Noske
    include/dubins.h #Andrew Walker
    include/dynamicvoronoi.h #Boris Lau, Christoph Sprunk, Wolfram Burgard
    include/bucketedqueue.h #Boris Lau, Christoph Sprunk, Wolfram Burgard
    include/point.h #Boris Lau, Christoph Sprunk, Wolfram Burgard
    )

set(SOURCES
    src/algorithm.cpp
    src/node2d.cpp
    src/node3d.cpp
    src/collisiondetection.cpp
    src/planner.cpp
    src/path.cpp
    src/smoother.cpp
    src/visualize.cpp
    src/dubins.cpp #Andrew Walker
    src/dynamicvoronoi.cpp #Boris Lau, Christoph Sprunk, Wolfram Burgard
    src/bucketedqueue.cpp #Boris Lau, Christoph Sprunk, Wolfram Burgard
    )


add_library(HYAS ${SOURCES} ${HEADERS})

include_directories(include ${catkin_INCLUDE_DIRS})
include_directories(include ${OMPL_INCLUDE_DIRS})
include_directories(include include)