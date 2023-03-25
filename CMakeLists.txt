cmake_minimum_required(VERSION 3.0)

set(QUICKJS
    "quickjs.c"
    "cutils.c"
    "libregexp.c"
    "libunicode.c"
)

set(QUICKJS_VERSION "2021-03-27")

if(NOT ESP_PLATFORM)
    option(QUICKJS_ENABLE_BIGNUM_EXT "Enable QuickJS Bignum extensions" OFF)

    if(QUICKJS_ENABLE_BIGNUM_EXT)
        list(APPEND QUICKJS "libbf.c")
    endif()

    add_library(quickjs STATIC ${QUICKJS})
    target_include_directories(quickjs PUBLIC .)
    target_compile_definitions(quickjs PUBLIC -DCONFIG_VERSION="${QUICKJS_VERSION}")

    if(QUICKJS_ENABLE_BIGNUM_EXT)
        target_compile_definitions(quickjs PUBLIC -DCONFIG_BIGNUM)
    endif()

else()
    if(CONFIG_QUICKJS_ENABLE_BIGNUM_EXT)
        list(APPEND QUICKJS "libbf.c")
    endif()

    idf_component_register(
        SRCS ${QUICKJS}
        INCLUDE_DIRS "."
    )

    target_compile_definitions(${COMPONENT_LIB} PUBLIC -DCONFIG_VERSION="${QUICKJS_VERSION}")

    if(CONFIG_QUICKJS_ENABLE_BIGNUM_EXT)
        target_compile_definitions(${COMPONENT_LIB} PUBLIC -DCONFIG_BIGNUM)
    endif()

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
