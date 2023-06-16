cmake_minimum_required(VERSION 3.4)

project(hello_world VERSION 0.0.1)

add_executable(hello_world hello_world.cpp)

set_target_properties(
	hello_world PROPERTIES
	CXX_STANDARD 11
	CXX_STANDARD_REQUIRED ON
)

target_include_directories(
	hello_world
	PRIVATE
	../formatter_ex_lib
)

target_link_libraries(hello_world formatter_ex_lib)
