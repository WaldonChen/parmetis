cmake_minimum_required(VERSION 2.8)
project(GKlib)

get_filename_component(abs "." ABSOLUTE)
set(GKLIB_PATH ${abs})
unset(abs)
include(GKlibSystem.cmake)

include_directories(".")
add_library(GKlib STATIC ${GKlib_sources})
set_target_properties(GKlib PROPERTIES
    VERSION ${METIS_MAJOR_VERSION}.${METIS_MINOR_VERSION}.${METIS_PATCH_VERSION}
    SOVERSION ${METIS_MAJOR_VERSION})

if(UNIX)
  target_link_libraries(GKlib m)
endif(UNIX)

include_directories("test")
add_subdirectory("test")

install(TARGETS GKlib
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib)
install(FILES ${GKlib_includes} DESTINATION include)
