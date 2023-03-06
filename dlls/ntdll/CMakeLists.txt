add_library(ntdll SHARED)
SET_TARGET_PROPERTIES(ntdll PROPERTIES PREFIX "")

set (CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Bsymbolic -Wl,-Bsymbolic-functions -static-libgcc -lunwind -lm ")

target_compile_definitions(ntdll
        PRIVATE
        #loader.o def
        SYSTEMDLLPATH=\"\"
        DLL_TO_BINDIR="`tools/makedep -R /usr/local/lib/wine /usr/local/bin`"
        BIN_TO_DATADIR="`tools/makedep -R /usr/local/bin /usr/local/share/wine`"
        BINDIR="/usr/local/bin"
        #thread.o def
        __WINESRC__
        _NTSYSTEM_
        _ACRTIMP=
        WINBASEAPI=
        WINE_UNIX_LIB)

target_include_directories(ntdll PRIVATE .)
target_include_directories(ntdll PRIVATE /usr/include/x86_64-linux-gnu)

#UNIX
file(GLOB_RECURSE UNIX_CPPs
        ${CMAKE_CURRENT_SOURCE_DIR}/unix/*.c
        )

target_sources(
        ntdll
        PRIVATE
        ${UNIX_CPPs}
)