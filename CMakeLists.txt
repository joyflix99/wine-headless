cmake_minimum_required(VERSION 3.22.1)

set(CMAKE_C_COMPILER "gcc")

add_compile_definitions(BIN_TO_DLLDIR "")
add_compile_definitions(
        WINE_UNIX_LIB
        __WINESRC__
        _NTSYSTEM_
        _REENTRANT
)

include_directories(/usr/include/x86_64-linux-gnu)
include_directories(include)
include_directories(wine)

add_subdirectory(dlls)
add_subdirectory(loader)

