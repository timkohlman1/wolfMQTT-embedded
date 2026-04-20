file(GLOB WOLFMQTT_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/src/*.c"
)

# Remove MQTT-SN - not needed for standard MQTT over TLS
list(REMOVE_ITEM WOLFMQTT_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/src/mqtt_sn_client.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/mqtt_sn_packet.c"
)

idf_component_register(
    SRCS ${WOLFMQTT_SOURCES}
    INCLUDE_DIRS "."
    REQUIRES wolfssl lwip
)

# Force-include lwip errno so EAGAIN/EWOULDBLOCK resolve in mqtt_socket.c
target_compile_options(__idf_wolfMQTT PRIVATE
    "-include" "lwip/errno.h"
)