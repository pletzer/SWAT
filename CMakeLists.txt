# ----------------------------------------------------------------------------------------------
#  Root CMake file for origin official SWAT and its variants developed by community.
#     Soil and Water Assessment Tool,
cmake_minimum_required(VERSION 3.20)

### Read SWAT versions and set project name
file(READ "VERSIONS" ver)
string(REGEX MATCH "VERSION_MAJOR ([0-9]*)" _ ${ver})  # major version, e.g., 2009 and 2012
set(SWAT_VER_MAJOR ${CMAKE_MATCH_1})
string(REGEX MATCH "VERSION_MINOR ([0-9]*)" _ ${ver})
set(SWAT_VER_MINOR ${CMAKE_MATCH_1})

set(SWAT_VER_STR ${SWAT_VER_MAJOR}.${SWAT_VER_MINOR})
set(prjname SWAT${SWAT_VER_MAJOR}Rev${SWAT_VER_MINOR})

if(${ver} MATCHES "VERSION_PATCH ([a-zA-Z0-9_.-]*)")
    set(SWAT_VER_PATCH ${CMAKE_MATCH_1})
    set(SWAT_VER_STR ${SWAT_VER_MAJOR}.${SWAT_VER_MINOR}.${SWAT_VER_PATCH})
    set(prjname SWAT${SWAT_VER_MAJOR}Rev${SWAT_VER_MINOR}-${SWAT_VER_PATCH})
endif()

message("Compiling SWAT version: ${SWAT_VER_STR}")

project(${prjname} Fortran)
set(CMAKE_VERBOSE_MAKEFILE ON)

enable_language(Fortran)

if (CMAKE_Fortran_COMPILER_ID STREQUAL "GNU")
    set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -ffixed-line-length-80")
elseif (CMAKE_Fortran_COMPILER_ID STREQUAL "Intel")
    set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -80")
elseif (CMAKE_Fortran_COMPILER_ID STREQUAL "Cray")
    set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -N80")
endif()

### Add subdirectories.
ADD_SUBDIRECTORY(src)
