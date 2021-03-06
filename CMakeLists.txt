﻿cmake_minimum_required(VERSION 3.0)
project(happy_ip)

# 清空build目录，保证CMakeCache.txt为最新
file(REMOVE_RECURSE ${PROJECT_SOURCE_DIR}/build/*)
message(STATUS "clean build file")

#debug或release模式
if(NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE "Release")
endif()
message(STATUS "Build " ${CMAKE_BUILD_TYPE})

add_compile_options(-std=c++14)
message(STATUS "optional: -std=c++14")

if (NOT DEFINED BOOST_INCLUDE_DIR)
	message( FATAL_ERROR "not define BOOST_INCLUDE_DIR" )
endif()
include_directories(${BOOST_INCLUDE_DIR} ./)

option(GLOG_OUTPUT OFF)
if (GLOG_OUTPUT)
	message (STATUS "build with glog")
	add_definitions(-DGLOG_OUTPUT -DGLOG_NO_ABBREVIATED_SEVERITIES -DGOOGLE_GLOG_DLL_DECL=)
endif()

add_subdirectory(src)
option(BUILD_EXAMPLES OFF)
if (BUILD_EXAMPLES)
	message (STATUS "build examples")
	add_subdirectory(examples)
endif()

install(FILES ${PROJECT_SOURCE_DIR}/singleton.h ${PROJECT_SOURCE_DIR}/thread_pool.h
		${PROJECT_SOURCE_DIR}/config_glog.h DESTINATION ${PROJECT_SOURCE_DIR}/install/include)
