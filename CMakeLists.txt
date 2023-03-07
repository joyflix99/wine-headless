cmake_minimum_required(VERSION 3.22.1)

set(CMAKE_C_COMPILER "gcc")

function(SET_OPTIONS_FUNC)
    #Options
    option(enable-win64 "build a Win64 emulator on AMD64 (won't run Win32 binaries)")
    option(with-wine64 "use the 64-bit Wine in DIR for a Wow64 build")
endfunction()

SET_OPTIONS_FUNC()

if (enable-win64)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m64 ")
else ()
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m32 ")
endif ()

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${BIT_FLAG} ")

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
add_subdirectory(server)

