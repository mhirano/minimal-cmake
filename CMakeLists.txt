cmake_minimum_required(VERSION 3.22)

project( MINIMAL CXX )

######## ######## ######## ######## ######## ######## ######## ########
# Compiler configuration
######## ######## ######## ######## ######## ######## ######## ########

# Check C++17 support and activate
include(CheckCXXCompilerFlag)
if(UNIX)
	CHECK_CXX_COMPILER_FLAG("-std=c++17" COMPILER_SUPPORTS_CXX17)
	if(COMPILER_SUPPORTS_CXX17)
		set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")
		set(CMAKE_CXX_STANDARD 17) # C++17...
		set(CMAKE_CXX_STANDARD_REQUIRED ON) #...is required...
		set(CMAKE_CXX_EXTENSIONS OFF) #...without compiler extensions like gnu++11
		message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has C++17 support.")
	else()
		message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++17 support. Please use a different C++ compiler.")
	endif()
elseif(MSVC) # Use Visual Studio as a compiler which comes with C++17!
	CHECK_CXX_COMPILER_FLAG("/std:c++17" COMPILER_SUPPORTS_CXX17)
	if (COMPILER_SUPPORTS_CXX17)
		add_compile_options("/std:c++17")
		message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has C++17 support.")
	else()
		message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++17 support. Please use a different C++ compiler.")
	endif()
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /DDEBUG")
endif()
add_compile_definitions("_SILENCE_STDEXT_ARR_ITERS_DEPRECATION_WARNING") # To supress C4996 warning

message(STATUS "CMAKE_CXX_FLAGS: ${CMAKE_CXX_FLAGS}")
message(STATUS "CMAKE_CXX_FLAGS_DEBUG: ${CMAKE_CXX_FLAGS_DEBUG}")
message(STATUS "CMAKE_CXX_FLAGS_RELEASE: ${CMAKE_CXX_FLAGS_RELEASE}")

if(MSVC)
	set(WIN32_FLAG WIN32)
endif()
######## ######## ######## ######## ######## ######## ######## ########

######## ######## ######## ######## ######## ######## ######## ########
# Adding executable
######## ######## ######## ######## ######## ######## ######## ########
add_executable(${PROJECT_NAME} ${WIN32_FLAG}
		${PROJECT_SOURCE_DIR}/src/main.cpp
)
######## ######## ######## ######## ######## ######## ######## ########

######## ######## ######## ######## ######## ######## ######## ########
# Linking libraries
######## ######## ######## ######## ######## ######## ######## ########
set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
target_include_directories(${PROJECT_NAME} PRIVATE ${PROJECT_SOURCE_DIR}/include)

## Threads
find_package(Threads REQUIRED)
target_link_libraries(${PROJECT_NAME} PRIVATE Threads::Threads)
######## ######## ######## ######## ######## ######## ######## ########
