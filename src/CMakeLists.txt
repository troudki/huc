cmake_minimum_required (VERSION 3.1)

# Force build type to Debug if none was specified.
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
endif(NOT CMAKE_BUILD_TYPE)

if(CMAKE_BUILD_TYPE MATCHES "Debug")
    add_definitions( -DDEBUG )
endif(CMAKE_BUILD_TYPE MATCHES "Debug")

find_package(Git)
if(GIT_FOUND)
    execute_process(
        COMMAND ${GIT_EXECUTABLE} describe --long --tags --dirty --always
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
        OUTPUT_VARIABLE GIT_VERSION
        ERROR_QUIET
        OUTPUT_STRIP_TRAILING_WHITESPACE
    )
    execute_process(
        COMMAND ${GIT_EXECUTABLE} log -1 --date=format:"%Y/%m/%d %T" --format=%ad
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
        OUTPUT_VARIABLE GIT_DATE
        ERROR_QUIET
        OUTPUT_STRIP_TRAILING_WHITESPACE
    )
else()
    set(GIT_VERSION "Unknown")
    set(GIT_DATE "__DATE__")
endif()
message( STATUS "GIT_VERSION: ${GIT_VERSION}")
message( STATUS "GIT_DATE: ${GIT_DATE}")
 
set(LIBRARY_OUTPUT_PATH    "${CMAKE_BINARY_DIR}/${CMAKE_BUILD_TYPE}")
set(EXECUTABLE_OUTPUT_PATH "${CMAKE_BINARY_DIR}/${CMAKE_BUILD_TYPE}")
set(ARCHIVE_OUTPUT_PATH    "${CMAKE_BINARY_DIR}/${CMAKE_BUILD_TYPE}")

add_subdirectory(huc)
add_subdirectory(mkit/as)