cmake_minimum_required(VERSION 2.8)
project(ParMETIS)

set(PARMETIS_MAJOR_VERSION 4)
set(PARMETIS_MINOR_VERSION 0)
set(PARMETIS_PATCH_VERSION 3)

set(GKLIB_PATH ${CMAKE_SOURCE_DIR}/metis/GKlib CACHE PATH "path to GKlib")
set(METIS_PATH ${CMAKE_SOURCE_DIR}/metis CACHE PATH "path to METIS")

# Symlink ./metis to wherever metis is. This allows files to be
# included from metis/libmetis/.
# execute_process(COMMAND ${CMAKE_COMMAND} -E create_symlink ${METIS_PATH} metis)

# Search for MPI.
# GK commented this out as it seems to be creating problems
include(FindMPI)
if(NOT MPI_FOUND)
  message(FATAL_ERROR "mpi is not found")
endif()
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${MPI_COMPILE_FLAGS}")

# Prepare libraries.
if(SHARED)
  set(ParMETIS_LIBRARY_TYPE SHARED)
else()
  set(ParMETIS_LIBRARY_TYPE STATIC)
endif()

include(${GKLIB_PATH}/GKlibSystem.cmake)

# List of paths that the compiler will search for header files.
# i.e., the -I equivalent
include_directories(include)
include_directories(${MPI_INCLUDE_PATH})
include_directories(${GKLIB_PATH})
include_directories(${METIS_PATH}/include)

# List of directories that cmake will look for CMakeLists.txt
if (METIS_INSTALL)
add_subdirectory(metis)
else()
FIND_PATH(METIS_INCLUDE_DIR metis.h
  /usr/include
  $ENV{METIS_PATH}/include
  )

FIND_LIBRARY(METIS_LIBRARY metis
  /usr/lib
  $ENV{METIS_PATH}/lib
  )

IF(METIS_INCLUDE_DIR)
  MESSAGE( STATUS, "Using already installed metis")
  IF(METIS_LIBRARY)
    SET( METIS_LIBRARIES ${PARMETIS_LIBRARY} ${METIS_LIBRARY})
    SET( METIS_FOUND "YES" )
  ENDIF(METIS_LIBRARY)
ENDIF(METIS_INCLUDE_DIR)

endif()
add_subdirectory(libparmetis)
add_subdirectory(include)
add_subdirectory(programs)

# This is for testing during development and is not being distributed
#add_subdirectory(test)
