add_custom_target(widl
        WORKING_DIRECTORY ${GNU_BUILD}/tools/widl
        COMMAND make -j 6
        COMMENT "BUILDING Widl Tool")