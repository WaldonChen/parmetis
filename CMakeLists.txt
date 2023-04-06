cmake_minimum_required(VERSION 3.0)

project(ParMETIS)

option(IDX64 "Use 64bit index value" OFF)
option(BUILD_PROGRAMS "Build parmetis programs" OFF)
option(BUILD_SHARED_LIBS "Build a shared library" OFF)

# Search for MPI.
find_package(MPI REQUIRED)
if(NOT MPI_FOUND)
  message(FATAL_ERROR "MPI is not found")
endif()

include(GNUInstallDirs)
set(INCLUDE_INSTALL_DIR ${CMAKE_INSTALL_INCLUDEDIR})

# set(GKLIB_PATH "${PROJECT_SOURCE_DIR}/metis/GKlib")
# include(${GKLIB_PATH}/GKlibSystem.cmake)

# List of directories that cmake will look for CMakeLists.txt
add_subdirectory(metis)
add_subdirectory(libparmetis)

if(BUILD_PROGRAMS)
  add_subdirectory(programs)
endif()

# This is for testing during development and is not being distributed
# add_subdirectory(test)

include(CMakePackageConfigHelpers)
configure_package_config_file(cmake/parmetis-config.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/parmetis-config.cmake
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake
  PATH_VARS INCLUDE_INSTALL_DIR)
write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/parmetis-config-version.cmake
  VERSION 4.0.3
  COMPATIBILITY SameMajorVersion)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/parmetis-config.cmake
              ${CMAKE_CURRENT_BINARY_DIR}/parmetis-config-version.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake)

install(TARGETS parmetis EXPORT parmetis-targets)
# install(TARGETS metis EXPORT metis-targets)
install(EXPORT parmetis-targets FILE parmetis-targets.cmake DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake)
# install(EXPORT metis-targets FILE metis-targets.cmake DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake)
install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/parmetis-config-version.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/parmetis-config.cmake
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake)
