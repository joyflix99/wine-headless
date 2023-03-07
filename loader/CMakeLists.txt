add_executable(wine)

add_dependencies(wine ntdll)
target_compile_definitions(wine
        PRIVATE
        #main.o def
        __WINESRC__
        DLLDIR="/usr/local/lib/wine"
        BIN_TO_DLLDIR="`tools/makedep`")

target_sources(wine
        PRIVATE
        main.c)

target_include_directories(wine
        PRIVATE
        ${CMAKE_SOURCE_DIR}/include)