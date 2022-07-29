project(SPCDNS LANGUAGES CXX)
cmake_minimum_required(VERSION 3.13.0)

include(Version.cmake)

set(${PROJECT_NAME}_INCLUDE
    src/dns.h
    src/mappings.h
    src/output.h)

set(${PROJECT_NAME}_SOURCE
    src/codec.c
    src/mappings.c
    src/output.c
)

configure_file(Resource.rc.in Resource.rc @ONLY)
set_source_files_properties(${CMAKE_CURRENT_BINARY_DIR}/Resource.rc PROPERTIES GENERATED 1)
set(${PROJECT_NAME}_RESOURCE
    ${CMAKE_CURRENT_BINARY_DIR}/Resource.rc
)

source_group("resource" FILES ${${PROJECT_NAME}_RESOURCE})
source_group("include" FILES ${${PROJECT_NAME}_INCLUDE})
source_group("source" FILES ${${PROJECT_NAME}_SOURCE})

add_library(${PROJECT_NAME} SHARED
    ${${PROJECT_NAME}_INCLUDE}
    ${${PROJECT_NAME}_SOURCE}
    ${${PROJECT_NAME}_RESOURCE}
)

target_compile_options(${PROJECT_NAME}
    PRIVATE
        $<$<CXX_COMPILER_ID:MSVC>:
            /MP /W3 /Zf>
)

target_include_directories(${PROJECT_NAME}
    PUBLIC
        ${PROJECT_SOURCE_DIR}/src
)

target_link_libraries(${PROJECT_NAME}
    PUBLIC
        Ws2_32
        iphlpapi
        Ntdll
)