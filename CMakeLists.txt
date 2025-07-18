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
)

find_package(Boost REQUIRED)

if(TARGET Boost::headers)
    set(_boost_headers_target Boost::headers)
elseif(TARGET Boost::boost)
    set(_boost_headers_target Boost::boost)
else()
    add_library(BoostHeaders INTERFACE IMPORTED)
    set_target_properties(BoostHeaders PROPERTIES
        INTERFACE_INCLUDE_DIRECTORIES "${Boost_INCLUDE_DIRS}")
    set(_boost_headers_target BoostHeaders)
endif()

find_package(Boost QUIET COMPONENTS crc)

if(NOT TARGET Boost::crc)
    add_library(Boost::crc INTERFACE IMPORTED)
    target_link_libraries(Boost::crc INTERFACE "${_boost_headers_target}")
endif()

add_library(noch-lib SHARED ${NOCH_SOURCES})
target_include_directories(noch-lib PUBLIC ${NOCH_INCLUDE_DIRS})
target_link_libraries(noch-lib PUBLIC Boost::crc)

if (MSVC)
  set_target_properties(noch-lib PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS ON)
endif()

install(DIRECTORY ${NOCH_INCLUDE_DIRS} DESTINATION include)


add_subdirectory(examples)

