file(GLOB WOLFMQTT_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/src/*.c"
)

list(REMOVE_ITEM WOLFMQTT_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/src/mqtt_sn_client.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/mqtt_sn_packet.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/mqtt_broker.c"
)

idf_component_register(
    SRCS ${WOLFMQTT_SOURCES}
    INCLUDE_DIRS "."
    REQUIRES wolfssl lwip freertos
)

target_compile_options(${COMPONENT_LIB} PRIVATE
    "SHELL:-include lwip/errno.h"
    "SHELL:-include wolfssl/wolfcrypt/settings.h"
)

target_compile_definitions(${COMPONENT_LIB} PRIVATE
    WOLFSSL_USER_SETTINGS
)