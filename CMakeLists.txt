cmake_minimum_required(VERSION 3.0)
set(app_path ${CMAKE_CURRENT_SOURCE_DIR})
get_filename_component(app_name ${app_path} NAME)
project(${app_name})

#some definitions for user
if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    set(MACOS 1)
elseif (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    set(LINUX 1)
elseif (${CMAKE_SYSTEM_NAME} MATCHES "Windows")
    set(WINDOWS 1)
endif ()

if (WINDOWS)
    set(home_path $ENV{HOMEPATH})
else ()
    set(home_path $ENV{HOME})
endif ()

# app config user specified
include(app_config.cmake) 

# if al_path not defined by run script, set to one specified by user config
if (NOT al_path)
	set(al_path ${user_al_path})
endif (NOT al_path)

# app.cmake script needs following to be predefined:
#         al_path
#         app_name
#         app_path
#         app_files_list
#         app_include_dirs (optional)
#         app_link_libs (optional)
include(${al_path}/cmake/app.cmake)

#set(VTK_DIR "/home/andres/Documents/src/vtk-build")

find_package(VTK REQUIRED)
include(${VTK_USE_FILE})
target_link_libraries("${app_name}" ${VTK_LIBRARIES})