# Add this directory for internal users.
include_directories(.)
# Find sources.
file(GLOB metis_sources *.c)
# Build libmetis.
add_library(metis SHARED ${GKlib_sources} ${metis_sources})
add_library(metis_static STATIC ${GKlib_sources} ${metis_sources})

set_target_properties(metis_static PROPERTIES OUTPUT_NAME metis)
if(UNIX)
  target_link_libraries(metis m)
  target_link_libraries(metis_static m)
endif()

if(METIS_INSTALL)
  install(TARGETS metis metis_static
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION lib
    ARCHIVE DESTINATION lib)
endif()
