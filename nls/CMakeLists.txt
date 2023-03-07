#NLS_SOURCE *.nls
file(GLOB_RECURSE NLS_FILES
        ${CMAKE_CURRENT_SOURCE_DIR}/*.nls
        )

ADD_CUSTOM_TARGET(
        NLS_SYMBOLIC_LINK
        DEPENDS NLS_SYMBOLIC_LINK_CMD)

add_custom_command(
        OUTPUT NLS_SYMBOLIC_LINK_CMD
        COMMAND ${CMAKE_COMMAND} -E echo "NLS_SYMBOLIC_LINK_CMD")

foreach(NLS_FILE ${NLS_FILES})
    get_filename_component(NLS_FILE_NAME ${NLS_FILE} NAME)
    add_custom_command(
            OUTPUT NLS_SYMBOLIC_LINK_CMD APPEND
            COMMAND ${CMAKE_COMMAND} -E create_symlink ${CMAKE_CURRENT_SOURCE_DIR}/${NLS_FILE_NAME} ${CMAKE_CURRENT_BINARY_DIR}/${NLS_FILE_NAME}
    )
endforeach()

