# Find sources.
aux_source_directory(. parmetis_sources)
# Create libparmetis
add_library(parmetis ${parmetis_sources})
# Link with metis and MPI libraries.
target_link_libraries(parmetis PUBLIC MPI::MPI_C metis)
set_target_properties(parmetis PROPERTIES PUBLIC_HEADER ${CMAKE_CURRENT_SOURCE_DIR}/../include/parmetis.h)
# Include directories for library code.
target_include_directories(
  parmetis PUBLIC
  $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:$<INSTALL_PREFIX>/include>
  PRIVATE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)
