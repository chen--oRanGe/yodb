cmake_minimum_required(VERSION 2.6)

project(yodb CXX)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
endif()

set(CXX_FLAGS
    -g
    -Wall
    -march=native
    -rdynamic
    -std=c++0x
)

string(REPLACE ";" " " CMAKE_CXX_FLAGS "${CXX_FLAGS}")

set(CMAKE_CXX_COMPILER "g++")
set(CMAKE_CXX_FLAGS_DEBUG "-O2")

string(TOUPPER ${CMAKE_BUILD_TYPE} BUILD_TYPE)
message(STATUS "CXX_FLAGS = " ${CMAKE_CXX_FLAGS} " " ${CMAKE_CXX_FLAGS_${BUILD_TYPE}})

find_package(Boost REQUIRED)
find_package(GTest REQUIRED)

include_directories(${Boost_INCLUDE_DIRS})
include_directories(${PROJECT_SOURCE_DIR})
include_directories(${PROJECT_SOURCE_DIR}/include)

set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin) 

file(GLOB_RECURSE SOURCE 
     ${CMAKE_CURRENT_SOURCE_DIR} 
     ${CMAKE_CURRENT_SOURCE_DIR}/db/*.cc
     ${CMAKE_CURRENT_SOURCE_DIR}/sys/*.cc
     ${CMAKE_CURRENT_SOURCE_DIR}/fs/*.cc
     ${CMAKE_CURRENT_SOURCE_DIR}/util/*.cc
     ${CMAKE_CURRENT_SOURCE_DIR}/tree/*.cc
     ${CMAKE_CURRENT_SOURCE_DIR}/cache/*.cc
)

add_library(yodb ${SOURCE})
target_link_libraries(yodb pthread rt aio)
set_target_properties(yodb PROPERTIES OUTPUT_NAME yodb)
install(TARGETS yodb DESTINATION lib)

add_subdirectory(test)
