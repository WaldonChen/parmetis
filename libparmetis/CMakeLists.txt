# Include directories for library code.
include_directories(.)
# Find sources.
file(GLOB parmetis_sources *.c)
# Create libparmetis
add_library(parmetis SHARED ${parmetis_sources})
add_library(parmetis_static STATIC ${parmetis_sources})
set_target_properties(parmetis_static PROPERTIES OUTPUT_NAME parmetis)

# Link with metis and MPI libraries.
target_link_libraries(parmetis metis ${MPI_LIBRARIES})
set_target_properties(parmetis PROPERTIES LINK_FLAGS "${MPI_LINK_FLAGS}")
target_link_libraries(parmetis_static metis_static ${MPI_LIBRARIES})
set_target_properties(parmetis_static PROPERTIES LINK_FLAGS "${MPI_LINK_FLAGS}")

install(TARGETS parmetis parmetis_static
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib
  ARCHIVE DESTINATION lib)
