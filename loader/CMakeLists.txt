set(WINE_TARGET_NAME wine${BIT_PREFIX})

add_executable(${WINE_TARGET_NAME})

add_dependencies(${WINE_TARGET_NAME} ntdll)
target_compile_definitions(${WINE_TARGET_NAME}
        PRIVATE
        #main.o def
        __WINESRC__
        DLLDIR="/usr/local/lib/wine"
        BIN_TO_DLLDIR="`tools/makedep`")

target_sources(${WINE_TARGET_NAME}
        PRIVATE
        main.c)

target_include_directories(${WINE_TARGET_NAME}
        PRIVATE
        ${CMAKE_SOURCE_DIR}/include)