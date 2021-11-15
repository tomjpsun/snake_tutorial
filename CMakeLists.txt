cmake_minimum_required(VERSION 3.10)

# set the project name
project(snake)
add_executable( ${PROJECT_NAME} 01_hello_SDL.cpp )

# Find the SDL2 library
find_package(SDL2 REQUIRED)

# Add SDL2 into include directories
include_directories(${SDL2_INCLUDE_DIR})

# And in the linking
target_link_libraries(${PROJECT_NAME} ${SDL2_LIBRARIES})
