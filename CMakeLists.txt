cmake_minimum_required(VERSION 3.24)

project(OpenGL)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD_LIBRARY libc++)

# Suppress warnings of the deprecation of GLUT functions on macOS.
if(APPLE)
  add_compile_options(-Wno-deprecated-declarations)
endif()

# Find the packages we need and create imported targets for them.
find_package(OpenGL REQUIRED)
find_package(GLUT REQUIRED)

# Linux
# If not on macOS, we need GLEW.
if(UNIX AND NOT APPLE)
  find_package(GLEW REQUIRED)
endif()

# Add the executable Example to be built from the source files in the current directory.
add_executable(Example)

# Search all the .cpp files in the directory where CMakeLists.txt lies and set them to ${SOURCE_FILES}.
file(GLOB SOURCE_FILES "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp")

# Search all the .h files in the directory where CMakeLists.txt lies and set them to ${INCLUDE_FILES}.
file(GLOB INCLUDE_FILES "${CMAKE_CURRENT_SOURCE_DIR}/*.h")

# Specify the source files and header files for the executable.
target_sources(Example PRIVATE ${SOURCE_FILES})
target_include_directories(Example PRIVATE ${INCLUDE_FILES})

# Link the executable to the imported targets of the libraries.
target_link_libraries(Example PRIVATE OpenGL::GL OpenGL::GLU GLUT::GLUT)

# If not on macOS, link GLEW as well.
if(UNIX AND NOT APPLE)
  target_link_libraries(Example PRIVATE GLEW::GLEW)
endif()

