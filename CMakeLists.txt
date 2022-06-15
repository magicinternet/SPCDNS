project(SPCDNS LANGUAGES C)
cmake_minimum_required(VERSION 3.13.0)

set(${PROJECT_NAME}_INCLUDE
    src/dns.h
    src/mappings.h
    src/output.h)

set(${PROJECT_NAME}_SOURCE
    src/codec.c
    src/mappings.c
    src/output.c
)

source_group("include" FILES ${${PROJECT_NAME}_INCLUDE})
source_group("source" FILES ${${PROJECT_NAME}_SOURCE})

add_library(${PROJECT_NAME} SHARED
    ${${PROJECT_NAME}_INCLUDE}
    ${${PROJECT_NAME}_SOURCE}
)

# target_compile_definitions(${PROJECT_NAME}
#     PRIVATE
#         _CRT_SECURE_NO_WARNINGS
#         _UNICODE
#         UNICODE
#         NOMINMAX
# )

#TODO: pass from the outside
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_17)
target_compile_options(${PROJECT_NAME}
    PRIVATE
        $<$<CXX_COMPILER_ID:MSVC>:
            /MP /W3 /Zf> /Tc
)

target_include_directories(${PROJECT_NAME}
    PUBLIC
        ${${PROJECT_NAME}_INCLUDE}
)

target_link_libraries(${PROJECT_NAME}
    PUBLIC
        Ws2_32
        iphlpapi
        Ntdll
)

# target_link_options(${PROJECT_NAME}
#     PRIVATE
#         "/ignore:4099" # warning LNK4099: PDB '.pdb' was not found ...
# )