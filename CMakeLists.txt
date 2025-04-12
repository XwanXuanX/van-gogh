cmake_minimum_required(VERSION 3.16)
project(VAN_GOGH LANGUAGES CXX)

# Including custom cmake rules
include(cmake/clang-cxx-dev-tools.cmake)

# Set the C++ standard to C++23
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Add all source files
file(GLOB_RECURSE SOURCES ${CMAKE_SOURCE_DIR}/source/*.cpp)
add_executable(vangogh ${SOURCES})

# Compiler flags
target_compile_options(vangogh PRIVATE
    -std=c++23
    -fcolor-diagnostics
    -fansi-escape-codes
    -Wall
    -g
    -Wno-deprecated
)

# Include directories
include_directories(
    ${CMAKE_SOURCE_DIR}/dependencies/include
    ${CMAKE_SOURCE_DIR}/source
)

# Specify link directories
link_directories(${CMAKE_SOURCE_DIR}/dependencies/library)
# Link the GLFW dynamic library
target_link_libraries(vangogh PRIVATE
    ${CMAKE_SOURCE_DIR}/dependencies/library/libglfw.3.4.dylib
)

# Find and link frameworks
find_library(OPENGL OpenGL)
find_library(COCOA Cocoa)
find_library(IOKIT IOKit)
find_library(COREVIDEO CoreVideo)
find_library(COREFOUNDATION CoreFoundation)

if (NOT OPENGL)
    message(FATAL_ERROR "OPENGL not found")
endif()

if (NOT COCOA)
    message(FATAL_ERROR "COCOA not found")
endif()

if (NOT IOKIT)
    message(FATAL_ERROR "IOKIT not found")
endif()

if (NOT COREVIDEO)
    message(FATAL_ERROR "COREVIDEO not found")
endif()

if (NOT COREFOUNDATION)
    message(FATAL_ERROR "COREFOUNDATION not found")
endif()

target_link_libraries(vangogh PRIVATE ${OPENGL})
target_link_libraries(vangogh PRIVATE ${COCOA})
target_link_libraries(vangogh PRIVATE ${IOKIT})
target_link_libraries(vangogh PRIVATE ${COREVIDEO})
target_link_libraries(vangogh PRIVATE ${COREFOUNDATION})
