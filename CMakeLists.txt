cmake_minimum_required(VERSION 3.25)
project(Project VERSION 0.0.0 LANGUAGES C)
option(BUILD_SHARED_LIBS "Build Shared Library" ON)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/lib)
set(PROJECT_DEPENDENCY_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/third_party)

include(FetchContent)

add_executable(${PROJECT_NAME}
	src/main.c
)

# FetchContent_Declare(
# 	cstl_unic
# 	GIT_REPOSITORY https://github.com/mugatai/cstl-unic.git
# 	GIT_TAG        v0.1.2
# 	GIT_SHALLOW    TRUE
# 	GIT_PROGRESS   TRUE  # Shows download progress
# 	SOURCE_DIR ${PROJECT_DEPENDENCY_DIRECTORY}/unic
# )
#
# FetchContent_MakeAvailable(cstl_unic)

target_include_directories(${PROJECT_NAME}
	PRIVATE
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
)

# target_link_libraries(${PROJECT_NAME} PRIVATE cstl::unic)

set_target_properties(${PROJECT_NAME} PROPERTIES
	VERSION ${PROJECT_VERSION}
	C_STANDARD 99
	C_STANDARD_REQUIRED YES
	C_EXTENSION NO
	OUTPUT_NAME "${PROJECT_NAME}_${PROJECT_VERSION}"
)

if(CMAKE_C_COMPILER_ID MATCHES "GNU|Clang")

	target_compile_options(${PROJECT_NAME} PRIVATE
		-Wall
		-Wextra 
		-Wpedantic 
		-Werror
	)

elseif(MSVC)

    target_compile_options(${PROJECT_NAME} PRIVATE /W4)

endif()
