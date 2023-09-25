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
    set(BIT_PREFIX "64")
    set(MINGW32_GCC "x86_64-w64-mingw32-gcc")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m64 ")
else ()
    set(BIT_PREFIX "")
    set(MINGW32_GCC "i686-w64-mingw32-gcc")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m32 ")
endif ()

#Configure GNU AUTOTools
set(GNU_BUILD ${CMAKE_BINARY_DIR}/GNU_BUILD)
function(CONFIGURE_GNU_AUTOTOOLS)
    file(MAKE_DIRECTORY ${GNU_BUILD})

    add_custom_target(GNU_BUILD_TARGET
            WORKING_DIRECTORY ${GNU_BUILD}
            COMMAND ../../configure --enable-win64
            COMMENT "Configure GNU AUTOTools")
endfunction()
CONFIGURE_GNU_AUTOTOOLS()

add_compile_definitions(
        WINE_UNIX_LIB
        __WINESRC__
        _NTSYSTEM_
        _REENTRANT
)

set(WIDL_COMPILER ${GNU_BUILD}/tools/widl/widl)
add_subdirectory(tools)
add_subdirectory(programs)
add_subdirectory(nls)
add_subdirectory(dlls)
add_subdirectory(loader)
add_subdirectory(server)

