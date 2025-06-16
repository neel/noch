cmake_minimum_required(VERSION 3.6)

project(noch-examples)

find_package(Boost REQUIRED COMPONENTS headers)

add_executable(noch main.cpp)
target_link_libraries(noch PRIVATE noch-lib Boost::boost)

