cmake_minimum_required(VERSION 3.16)

# Project Name
project(http_server_challenge LANGUAGES CXX)

# Require Modern C++
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Output binary location: server/build/http_server
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

# Collect source files
file(GLOB_RECURSE SOURCES
    src/*.cpp
)

# Create executable
add_executable(http_server ${SOURCES})

# Enable warnings (important for systems programming)
if (MSVC)
    target_compile_options(http_server PRIVATE /W4)
else()
    target_compile_options(http_server PRIVATE
        -Wall
        -Wextra
        -Wpedantic
        -Wconversion
        -Wsign-conversion
        -Wshadow
    )
endif()

# Thread support (needed for Stage 09 concurrency)
find_package(Threads REQUIRED)
target_link_libraries(http_server PRIVATE Threads::Threads)

# Platform-specific networking libraries
if (WIN32)
    target_link_libraries(http_server PRIVATE ws2_32)
endif()
