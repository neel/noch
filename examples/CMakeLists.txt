cmake_minimum_required(VERSION 3.6)

project(noch-examples)

add_executable(noch main.cpp)
target_link_libraries(noch PRIVATE noch-lib)
