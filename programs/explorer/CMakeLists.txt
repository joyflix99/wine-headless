set(CMAKE_C_COMPILER ${MINGW32_GCC})
set(CMAKE_C_LINKER_LAUNCHER ${CMAKE_BINARY_DIR}/tools/winegcc/winegcc)

add_executable(explorer.exe)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -fno-strict-aliasing -Wdeclaration-after-statement -Wempty-body -Wignored-qualifiers -Winit-self -Wno-packed-not-aligned -Wshift-overflow=2 -Wstrict-prototypes -Wabsolute-value -Wformat-overflow -Wnonnull -mcx16 -gdwarf-4 -g -O2"
        )

#Explorer *.c
file(GLOB Explorer_Cs
        ${CMAKE_CURRENT_SOURCE_DIR}/*.c
        )

target_compile_definitions(explorer.exe
        PRIVATE
        _UCRT
        __WINESRC__
        __WINE_PE_BUILD
        )

target_sources(explorer.exe
        PRIVATE
        ${Explorer_Cs})

target_include_directories(explorer.exe
        PRIVATE
        .
        ${CMAKE_SOURCE_DIR}/include
        ${CMAKE_SOURCE_DIR}/include/msvcrt)
