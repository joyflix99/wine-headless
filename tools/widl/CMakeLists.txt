add_executable(widl)

#Bison
BISON_TARGET(PARSER.TAB parser.y parser.tab.c
        DEFINES_FILE ${CMAKE_CURRENT_BINARY_DIR}/parser.tab.h)

#FLEX
FLEX_TARGET(PARSER.YY parser.l parser.yy.c)
message("bison header:" ${BISON_PARSER.TAB_OUTPUT_HEADER})
#widl *.c
file(GLOB WIDL_Cs
        ${CMAKE_CURRENT_SOURCE_DIR}/*.c
        )

target_compile_definitions(widl
        PRIVATE
        __WINESRC__
        INCLUDEDIR="/usr/local/include"
        DLLDIR="/usr/local/lib/wine"
        BIN_TO_INCLUDEDIR="`tools/makedep -R /usr/local/bin /usr/local/include`"
        BIN_TO_DLLDIR="`tools/makedep -R /usr/local/bin /usr/local/lib/wine`"
        _FORTIFY_SOURCE=0
)

target_sources(widl
        PRIVATE
        ${BISON_PARSER.TAB_OUTPUTS}
        ${FLEX_PARSER.YY_OUTPUTS}
        ${WIDL_Cs}
        )

target_include_directories(widl
        PRIVATE
        .
        ${BISON_PARSER.TAB_OUTPUT_HEADER}
        ${CMAKE_SOURCE_DIR}/tools/wrc
        ${CMAKE_SOURCE_DIR}/include)