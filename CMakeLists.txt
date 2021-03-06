cmake_minimum_required (VERSION 3.10)

if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR})
    message(FATAL_ERROR "Prevented in-tree build.")
endif()

project (voxel
    VERSION 1.0.0
    LANGUAGES CXX
)

include(GNUInstallDirs)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_subdirectory(3rdparty/FastNoise)

set(FN_SET_c0rp3n_CXX_FLAGS OFF CACHE BOOL "")
set(FN_COMPILE_SSE2 ON CACHE BOOL "")
set(FN_COMPILE_SSE41 ON CACHE BOOL "")
set(FN_COMPILE_AVX2 ON CACHE BOOL "")
set(FN_COMPILE_AVX512 OFF CACHE BOOL "")
set(FN_USE_FMA ON CACHE BOOL "")
set(FN_ALIGNED_SETS ON CACHE BOOL "")
add_subdirectory(3rdparty/FastNoiseSIMD)

add_library(voxel INTERFACE)
target_link_libraries(voxel INTERFACE
    FastNoise
    FastNoiseSIMD
)
target_include_directories(voxel INTERFACE
    ${PROJECT_BINARY_DIR}
	${PROJECT_SOURCE_DIR}/include
)

