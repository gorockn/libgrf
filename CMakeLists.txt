cmake_minimum_required(VERSION 2.8)
project(libgrf C)

set(LIBGRF_MAJOR_VERSION 1)
set(LIBGRF_MINOR_VERSION 2)
set(LIBGRF_PATCH_VERSION 0)
set(LIBGRF_VERSION ${LIBGRF_MAJOR_VERSION}.${LIBGRF_MINOR_VERSION}.${LIBGRF_PATCH_VERSION})

set(INSTALL_LIB_DIR lib CACHE PATH "Installation directory for libraries")
set(INSTALL_PKGCONF_DIR lib/pkgconfig CACHE PATH "Installation directory for pkg-config files")
set(INSTALL_INCLUDE_DIR include CACHE PATH "Installation directory for header files")

find_package(ZLIB REQUIRED)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu89")

file(GLOB SRCS "${CMAKE_SOURCE_DIR}/src/*.c")
file(GLOB INCS "${CMAKE_SOURCE_DIR}/inc/*.h")

include_directories("${ZLIB_INCLUDE_DIRS}")
include_directories("${CMAKE_SOURCE_DIR}/inc")
add_library(grf_static STATIC ${SRCS} ${INCS})
add_library(grf_shared SHARED ${SRCS} ${INCS})
target_link_libraries(grf_static ${ZLIB_LIBRARIES})
target_link_libraries(grf_shared ${ZLIB_LIBRARIES})
set_target_properties(grf_static PROPERTIES OUTPUT_NAME grf)
set_target_properties(grf_shared PROPERTIES OUTPUT_NAME grf)
set_target_properties(grf_shared PROPERTIES IMPORT_SUFFIX _import.lib)
set_target_properties(grf_shared PROPERTIES VERSION ${LIBGRF_MAJOR_VERSION} SOVERSION ${LIBGRF_VERSION})
configure_file(${CMAKE_SOURCE_DIR}/cmake/libgrf.pc.in libgrf.pc @ONLY)

install(TARGETS grf_static DESTINATION ${INSTALL_LIB_DIR})
install(TARGETS grf_shared DESTINATION ${INSTALL_LIB_DIR})
install(FILES ${INCS} DESTINATION ${INSTALL_INCLUDE_DIR})
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/libgrf.pc DESTINATION ${INSTALL_PKGCONF_DIR})
