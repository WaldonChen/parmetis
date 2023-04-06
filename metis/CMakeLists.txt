cmake_minimum_required(VERSION 3.0)

project(METIS)

option(IDX64 "Use 64bit index value" OFF)
option(BUILD_PROGRAMS "Build parmetis programs" OFF)
option(BUILD_SHARED_LIBS "Build a shared library" OFF)

include(GNUInstallDirs)
set(INCLUDE_INSTALL_DIR ${CMAKE_INSTALL_INCLUDEDIR})

set(GKLIB_PATH "${CMAKE_CURRENT_SOURCE_DIR}/GKlib")
include(${GKLIB_PATH}/GKlibSystem.cmake)

if(IDX64)
  set(IDXTYPEWIDTH 64)
else()
  set(IDXTYPEWIDTH 32)
endif()
set(METIS_HEADER ${CMAKE_CURRENT_BINARY_DIR}/include/metis.h)
configure_file(include/metis.h.in ${METIS_HEADER} @ONLY)

add_subdirectory(libmetis)

if(BUILD_PROGRAMS)
  add_subdirectory(programs)
endif()

include(CMakePackageConfigHelpers)
configure_package_config_file(cmake/metis-config.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/metis-config.cmake
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake
  PATH_VARS INCLUDE_INSTALL_DIR)
write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/metis-config-version.cmake
  VERSION 4.0.3
  COMPATIBILITY SameMajorVersion)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/metis-config.cmake
              ${CMAKE_CURRENT_BINARY_DIR}/metis-config-version.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake)

install(TARGETS metis EXPORT metis-targets)
install(EXPORT metis-targets FILE metis-targets.cmake DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake)
install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/metis-config-version.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/metis-config.cmake
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake)
