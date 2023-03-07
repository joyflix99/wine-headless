add_executable(wineserver)

add_dependencies(wineserver NLS_SYMBOLIC_LINK)

target_compile_definitions(wineserver
        PRIVATE
        BIN_TO_NLSDIR="`${WITH_WINE64}/tools/makedep -R /usr/local/bin /usr/local/share/wine/nls`"
        NLSDIR="/usr/local/share/wine/nls")

#Server *.c
file(GLOB_RECURSE Server_Cs
        ${CMAKE_CURRENT_SOURCE_DIR}/*.c
        )

target_include_directories(wineserver
        PRIVATE
        .
        ${CMAKE_SOURCE_DIR}/include)

target_sources(wineserver
        PRIVATE
        ${Server_Cs})