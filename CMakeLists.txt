# CMake entry point
cmake_minimum_required (VERSION 3.0)
project (Perspective_Colored_Cube)

find_package(OpenGL REQUIRED)


if( CMAKE_BINARY_DIR STREQUAL CMAKE_SOURCE_DIR )
    message( FATAL_ERROR "Please select another Build Directory ! (and give it a clever name, like bin_Visual2012_64bits/)" )
endif()
if( CMAKE_SOURCE_DIR MATCHES " " )
	message( "Your Source Directory contains spaces. If you experience problems when compiling, this can be the cause." )
endif()
if( CMAKE_BINARY_DIR MATCHES " " )
	message( "Your Build Directory contains spaces. If you experience problems when compiling, this can be the cause." )
endif()



# Compile external dependencies 
add_subdirectory (external)

# On Visual 2005 and above, this module can set the debug working directory
cmake_policy(SET CMP0026 OLD)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/external/rpavlik-cmake-modules-fe2273")
include(CreateLaunchers)
include(MSVCMultipleProcessCompile) # /MP

if(INCLUDE_DISTRIB)
	add_subdirectory(distrib)
endif(INCLUDE_DISTRIB)



include_directories(
	external/AntTweakBar-1.16/include/
	external/glfw-3.1.2/include/
	external/glm-0.9.7.1/
	external/glew-1.13.0/include/
	external/assimp-3.0.1270/include/
	external/bullet-2.81-rev2613/src/
	.
)

set(ALL_LIBS
	${OPENGL_LIBRARY}
	glfw
	GLEW_1130
)

add_definitions(
	-DTW_STATIC
	-DTW_NO_LIB_PRAGMA
	-DTW_NO_DIRECT3D
	-DGLEW_STATIC
	-D_CRT_SECURE_NO_WARNINGS
)

add_executable(two_point_perspective
	two_point_perspective/two_point_perspective.cpp
	common/shader.cpp
	common/shader.hpp
	
	two_point_perspective/TransformVertexShader.vertexshader
	two_point_perspective/ColorFragmentShader.fragmentshader
)
target_link_libraries(two_point_perspective
	${ALL_LIBS}
)
# Xcode and Visual working directories
set_target_properties(two_point_perspective PROPERTIES XCODE_ATTRIBUTE_CONFIGURATION_BUILD_DIR "${CMAKE_CURRENT_SOURCE_DIR}/two_point_perspective/")
create_target_launcher(two_point_perspective WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/two_point_perspective/")

add_executable(three_point_perspective
	three_point_perspective/three_point_perspective.cpp
	common/shader.cpp
	common/shader.hpp

	three_point_perspective/TransformVertexShader.vertexshader
	three_point_perspective/ColorFragmentShader.fragmentshader
)
target_link_libraries(three_point_perspective
	${ALL_LIBS}
)
# Xcode and Visual working directories
set_target_properties(three_point_perspective PROPERTIES XCODE_ATTRIBUTE_CONFIGURATION_BUILD_DIR "${CMAKE_CURRENT_SOURCE_DIR}/three_point_perspective/")
create_target_launcher(three_point_perspective WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/three_point_perspective/")

SOURCE_GROUP(common REGULAR_EXPRESSION ".*/common/.*" )
SOURCE_GROUP(shaders REGULAR_EXPRESSION ".*/.*shader$" )


if (NOT ${CMAKE_GENERATOR} MATCHES "Xcode" )
add_custom_command(
   TARGET two_point_perspective POST_BUILD
   COMMAND ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_CFG_INTDIR}/two_point_perspective${CMAKE_EXECUTABLE_SUFFIX}" "${CMAKE_CURRENT_SOURCE_DIR}/two_point_perspective/"
)
add_custom_command(
   TARGET three_point_perspective POST_BUILD
   COMMAND ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_CFG_INTDIR}/three_point_perspective${CMAKE_EXECUTABLE_SUFFIX}" "${CMAKE_CURRENT_SOURCE_DIR}/three_point_perspective/"
)

elseif (${CMAKE_GENERATOR} MATCHES "Xcode" )

endif (NOT ${CMAKE_GENERATOR} MATCHES "Xcode" )

