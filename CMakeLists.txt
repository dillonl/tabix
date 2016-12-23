# =================================
# tabix
#
# tabix/CMakeLists.tx
# =================================

#Set up min CMake version
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

#Set project name
PROJECT(tabix)

SET(ZLIB_LIBRARY "" CACHE STRING "Some user-specified option")

if(ZLIB_LIBRARY STREQUAL "") 
  message("adding zlib to tabix")
  include(zlib.cmake)
endif()

INCLUDE_DIRECTORIES(${ZLIB_INCLUDE})

IF("${PROJECT_SOURCE_DIR}" STREQUAL "${PROJECT_BINARY_DIR}")
  MESSAGE(SEND_ERROR "In-source builds are not allowed, please build this in the bin directory!")
ENDIF("${PROJECT_SOURCE_DIR}" STREQUAL "${PROJECT_BINARY_DIR}")

set(TABIX_SOURCE
  bedidx.c
  bgzf.c
  index.c
  knetfile.c
  kstring.c
  main.c
)

set(BGZIP_SOURCE
  bgzf.c
  bgzip.c
)

set(TABIX_LIB_SOURCE
  main.c
  bgzip.c
  bedidx.c
  bgzf.c
  index.c
  knetfile.c
  kstring.c
)

add_library(libtabix STATIC
  ${TABIX_LIB_SOURCE}
)

set_target_properties(libtabix
  PROPERTIES
  PREFIX ""
)

#create binary
add_executable(tabix
  ${TABIX_SOURCE}
)

#create binary
add_executable(bgzip
  ${BGZIP_SOURCE}
)

target_link_libraries(tabix ${ZLIB_LIBRARY})
target_link_libraries(bgzip ${ZLIB_LIBRARY})

add_dependencies(tabix ${ZLIB_PROJECT})
add_dependencies(bgzip ${ZLIB_PROJECT})
