add_library(ntdll SHARED)

target_include_directories(ntdll PRIVATE .)

#UNIX
file(GLOB_RECURSE UNIX_CPPs
        ${CMAKE_CURRENT_SOURCE_DIR}/unix/*.c
        )

target_sources(
        ntdll
        PRIVATE
        ${UNIX_CPPs}
)