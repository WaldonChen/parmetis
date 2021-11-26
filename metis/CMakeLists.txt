cmake_minimum_required(VERSION 2.8)
project(METIS)

set(METIS_MAJOR_VERSION 5)
set(METIS_MINOR_VERSION 1)
set(METIS_PATCH_VERSION 0)

set(GKLIB_PATH "${PROJECT_SOURCE_DIR}/GKlib" CACHE PATH "path to GKlib")
set(SHARED FALSE CACHE BOOL "build a shared library")
include(GNUInstallDirs)

if(MSVC)
  set(METIS_INSTALL FALSE)
else()
  set(METIS_INSTALL TRUE)
endif()

# Configure libmetis library.
set(METIS_LIBRARY_TYPE)
if(SHARED)
  set(METIS_LIBRARY_TYPE SHARED)
else()
  set(METIS_LIBRARY_TYPE STATIC)
endif(SHARED)

include(${GKLIB_PATH}/GKlibSystem.cmake)
# Add include directories.
include_directories(${GKLIB_PATH})
include_directories(include)
# Recursively look for CMakeLists.txt in subdirs.
add_subdirectory("include")
add_subdirectory(GKlib)
add_subdirectory("libmetis")
add_subdirectory("programs")
