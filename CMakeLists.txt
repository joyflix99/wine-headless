cmake_minimum_required(VERSION 3.22.1)

project(Wine-CMake)

set(CMAKE_C_COMPILER "gcc")

function(SET_OPTIONS_FUNC)
    #Options
    option(enable-win64 "build a Win64 emulator on AMD64 (won't run Win32 binaries)")
    option(with-wine64 "use the 64-bit Wine in DIR for a Wow64 build")
endfunction()
function(FIND_PACKAGES)
    find_package(BISON)
    find_package(FLEX)
endfunction()

SET_OPTIONS_FUNC()

FIND_PACKAGES()

if (enable-win64)
    set(MINGW32_GCC "x86_64-w64-mingw32-gcc")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m64 ")
else ()
    set(MINGW32_GCC "i686-w64-mingw32-gcc")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m32 ")
endif ()

add_compile_definitions(
        WINE_UNIX_LIB
        __WINESRC__
        _NTSYSTEM_
        _REENTRANT
)

add_subdirectory(tools)
add_subdirectory(programs)
add_subdirectory(nls)
add_subdirectory(dlls)
add_subdirectory(loader)
add_subdirectory(server)

