cmake_minimum_required(VERSION 3.6)

project(noch)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

SET(NOCH_INCLUDE_DIRS
  ${CMAKE_CURRENT_SOURCE_DIR}/includes
)

SET(NOCH_SOURCES
    sources/detail/imurmur3.cpp
    sources/io.cpp
)

find_package(Boost REQUIRED COMPONENTS headers)

add_library(noch-lib SHARED ${NOCH_SOURCES})
target_include_directories(noch-lib PUBLIC ${NOCH_INCLUDE_DIRS})
target_link_libraries(noch-lib PRIVATE Boost::boost)

install(DIRECTORY ${NOCH_INCLUDE_DIRS} DESTINATION include)

add_subdirectory(examples)

