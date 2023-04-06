# Find sources.
aux_source_directory(. metis_sources)
# Build libmetis.
add_library(metis ${metis_sources} ${GKlib_sources})
if(UNIX)
  target_link_libraries(metis PUBLIC m)
endif()
set_target_properties(metis PROPERTIES PUBLIC_HEADER ${METIS_HEADER})
# Add this directory for internal users.
target_include_directories(
  metis PUBLIC
  $<BUILD_INTERFACE:${GKLIB_PATH}>
  # $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/metis/include>
  $<BUILD_INTERFACE:${PROJECT_BINARY_DIR}/include>
  $<INSTALL_INTERFACE:$<INSTALL_PREFIX>/include>
  PRIVATE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)

