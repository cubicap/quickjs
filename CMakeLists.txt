cmake_minimum_required(VERSION 3.0)

set(QUICKJS
    "quickjs.c"
    "cutils.c"
    "libbf.c"
    "libregexp.c"
    "libunicode.c"
)

if(NOT ESP_PLATFORM)
    add_library(quickjs STATIC ${QUICKJS})
    target_include_directories(quickjs PUBLIC .)
    target_compile_definitions(quickjs PUBLIC -DCONFIG_VERSION="2021-03-27")

else()
    idf_component_register(
        SRCS ${QUICKJS}
        INCLUDE_DIRS "."
    )

    target_compile_definitions(${COMPONENT_LIB} PUBLIC -DCONFIG_VERSION="2021-03-27")

    target_compile_options(${COMPONENT_LIB} PRIVATE
        -Wno-cast-function-type
        -Wno-format
        -Wno-implicit-fallthrough
        -Wno-incompatible-pointer-types
        -Wno-maybe-uninitialized
        -Wno-uninitialized
        -Wno-unused-value
    )

endif()
