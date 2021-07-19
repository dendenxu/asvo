# CMake configuration

cmake_minimum_required(VERSION 3.0.0)

# Project configuration

project(asvo_cuda)

file(GLOB headers "*.h")
file(GLOB sources "*.cpp" "*.cu")

if(MSVC)
	source_group("Header Files" FILES ${headers})
	source_group("Source Files" FILES ${sources})
endif()

find_package(CUDA REQUIRED)

# 3rd party dependencies

# I guess you'll have to go with absolute path

set(freeglut_inc_dir "${CMAKE_SOURCE_DIR}/3rd/freeglut/include" CACHE PATH
"Path to include directory of freeglut 3.0.0 or later. \
The include directory is the one containing the directory 'GL' (so you can write '#include <GL/...>').")
set(glew_inc_dir "${CMAKE_SOURCE_DIR}/3rd/glew-2.1.0/include" CACHE PATH
"Path to include directory of Glew 1.12.0 or later. \
The include directory is the one containing the directory 'GL' (so you can write '#include <GL/...>').")

set(freeglut_lib_debug "${CMAKE_SOURCE_DIR}/3rd/freeglut/lib/x64/freeglut.lib" CACHE FILEPATH
"Path to freeglut.lib version 3.0.0 or later.")
set(freeglut_lib_release "${CMAKE_SOURCE_DIR}/3rd/freeglut/lib/x64/freeglut.lib" CACHE FILEPATH
"Path to freeglutd.lib version 3.0.0 or later.")


set(glew_lib "${CMAKE_SOURCE_DIR}/3rd/glew-2.1.0/lib/Release/x64/glew32.lib" CACHE FILEPATH
"Path to glew32.lib release, single-context, dynamic, version 1.12.0 or later. \
Either 32bit or 64bit depending on how you intend to build this project.")

# Include paths

include_directories(
	${freeglut_inc_dir}
	${glew_inc_dir}
)

cuda_add_executable(demo ${headers} ${sources})

# Libraries

target_link_libraries(
    demo ${glew_lib}
	debug ${freeglut_lib_debug}
	optimized ${freeglut_lib_release}
)

# ? This will print all varibles defined here
# get_cmake_property(_variableNames VARIABLES)
# list (SORT _variableNames)
# foreach (_variableName ${_variableNames})
#     message(STATUS "${_variableName}=${${_variableName}}")
# endforeach()