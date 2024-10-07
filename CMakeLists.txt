cmake_minimum_required (VERSION 3.8)
project (mandelbrot)

find_package(Vulkan)

set (CMAKE_CXX_STANDARD 14)

include_directories(${Vulkan_INCLUDE_DIR})

add_executable(mandelbrot src/mandelbrot.cc src/lodepng.cpp src/vulkan_ext.c)

target_link_libraries(mandelbrot ${Vulkan_LIBRARY})

file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/shaders)

add_custom_target(shader
	ALL
	DEPENDS shaders/comp.spv
)

add_custom_command(
	OUTPUT shaders/comp.spv
	COMMAND glslc ${CMAKE_SOURCE_DIR}/shaders/shader.comp -o shaders/comp.spv
	MAIN_DEPENDENCY ${CMAKE_SOURCE_DIR}/shaders/shader.comp
)
