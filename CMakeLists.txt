cmake_minimum_required(VERSION 3.10)

# set the project name
project(snake)

add_executable( ${PROJECT_NAME} 06_extension_libraries_and_loading_other_image_formats.cpp)
# Find the SDL2 library
find_package(SDL2 REQUIRED)

# Add SDL2 into include directories
include_directories(${SDL2_INCLUDE_DIR})

# Add SDL link library
target_link_libraries(${PROJECT_NAME} ${SDL2_LIBRARIES})

# Define Project directory
add_compile_definitions("PROJECT_DIR=\"${PROJECT_SOURCE_DIR}\"")

target_link_libraries(${PROJECT_NAME} -lSDL2_image)
target_link_libraries(${PROJECT_NAME} -lSDL2_ttf)
target_link_libraries(${PROJECT_NAME} -lSDL2_net)
target_link_libraries(${PROJECT_NAME} -lSDL2_mixer)
