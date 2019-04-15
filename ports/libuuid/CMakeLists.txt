cmake_minimum_required(VERSION 3.0)
project(libuuid C)

configure_file(config.linux.h config.h COPYONLY)

include_directories(${CMAKE_CURRENT_BINARY_DIR})

message(STATUS "VCPKG_LIBRARY_LINKAGE=${VCPKG_LIBRARY_LINKAGE}")
if(VCPKG_LIBRARY_LINKAGE STREQUAL "static")
	set(UUID_LIB_MODE STATIC)
else()
	set(UUID_LIB_MODE SHARED)
endif()

add_library(uuid ${UUID_LIB_MODE}
    clear.c
    compare.c
    copy.c
    gen_uuid.c
    isnull.c
    pack.c
    parse.c
    randutils.c
    unpack.c
    unparse.c
    uuid_time.c
)
target_compile_options(uuid PRIVATE -include "${CMAKE_CURRENT_BINARY_DIR}/config.h")

add_executable(test_uuid test_uuid.c)
target_link_libraries(test_uuid uuid)

if(CMAKE_BUILD_TYPE STREQUAL "Release")
    install(FILES uuid.h DESTINATION include/uuid)
endif()

install(
    TARGETS uuid
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)
