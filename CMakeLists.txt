# =================================
# tabix
#
# tabix/CMakeLists.tx
# =================================

#Set up min CMake version
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

#Set project name
PROJECT(tabix)

option (ZLIB_LIBRARY "Use tutorial provided math implementation" OFF)
option (ZLIB_INCLUDE "Use tutorial provided math implementation" OFF)

if (NOT ZLIB_LIBRARY)
  include(zlib.cmake)
  set(ADD_ZLIB_PROJECT TRUE)
endif()

INCLUDE_DIRECTORIES(${ZLIB_INCLUDE} ${PROJECT_SOURCE_DIR})

IF("${PROJECT_SOURCE_DIR}" STREQUAL "${PROJECT_BINARY_DIR}")
  MESSAGE(SEND_ERROR "In-source builds are not allowed, please build this in the bin directory!")
ENDIF("${PROJECT_SOURCE_DIR}" STREQUAL "${PROJECT_BINARY_DIR}")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

set(TABIX_SOURCE
  main.c
)

set(BGZIP_SOURCE
  bgzf.c
  bgzip.c
)

set(TABIX_LIB_SOURCE
  bgzip.c
  bgzf.c
  index.c
  bedidx.c
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
#add_executable(tabix
#  ${TABIX_SOURCE}
#)

#create binary
add_executable(bgzip
  ${BGZIP_SOURCE}
)

target_link_libraries(libtabix ${ZLIB_LIBRARY})
target_link_libraries(bgzip ${ZLIB_LIBRARY})
#target_link_libraries(tabix libtabix ${ZLIB_LIBRARY})


if (ADD_ZLIB_PROJECT)
  add_dependencies(libtabix ${ZLIB_PROJECT})
  add_dependencies(bgzip ${ZLIB_PROJECT})
#  add_dependencies(tabix libtabix ${ZLIB_PROJECT})
endif()
