cmake_minimum_required(VERSION 2.8)
project(libgrf)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu89")

set(src_dir "${CMAKE_SOURCE_DIR}/src")
set(inc_dir "${CMAKE_SOURCE_DIR}/inc")
file(GLOB src_files "${src_dir}/*.c")
file(GLOB inc_files "${inc_dir}/*.h")

set(version 1)
set(soversion 1.2.0)

include_directories("${inc_dir}")
add_library(grf_static SHARED ${src_files})
add_library(grf_shared STATIC ${src_files})
set_target_properties(grf_static PROPERTIES OUTPUT_NAME grf)
set_target_properties(grf_shared PROPERTIES OUTPUT_NAME grf)
set_target_properties(grf_shared PROPERTIES VERSION ${version} SOVERSION ${soversion})

install(TARGETS grf_static DESTINATION lib)
install(TARGETS grf_shared DESTINATION lib)
install(FILES ${inc_files} DESTINATION include)