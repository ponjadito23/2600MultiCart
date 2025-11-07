cmake_minimum_required(VERSION 3.21)

# Pull in SDK (must be before project)
# include(pico_sdk_import.cmake)
include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)

# project(PICO_ROM CXX C ASM)
project(PICO_ROM CXX C ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_C_FLAGS_RELEASE_INIT "-Os")
set(CMAKE_CXX_FLAGS_RELEASE_INIT "-Os")

# Initialize the SDK
pico_sdk_init()

## Pico with WIFI

add_executable(pico_rom_wifi)

target_compile_definitions(pico_rom_wifi PRIVATE
            WIFI_SSID=\"${WIFI_SSID}\"
            WIFI_PASSWORD=\"${WIFI_PASSWORD}\"
            TEST_TCP_SERVER_IP=\"${TEST_TCP_SERVER_IP}\"
            )

target_sources(pico_rom_wifi PRIVATE rom_wifi.c)

target_include_directories(pico_rom_wifi PRIVATE
            ${CMAKE_CURRENT_LIST_DIR}
            ${CMAKE_CURRENT_LIST_DIR}/..
            )

target_link_libraries(pico_rom_wifi PRIVATE
        pico_stdlib
        hardware_pio
        hardware_adc
	pico_cyw43_arch_lwip_threadsafe_background     # Specific for PICO_W
        pico_lwip_mqtt
        pico_multicore
        )

pico_add_extra_outputs(pico_rom_wifi)

# Enable USB output, disable UART output.
pico_enable_stdio_usb(pico_rom_wifi 1)
pico_enable_stdio_uart(pico_rom_wifi 0)


## PICO no WIFI version

add_executable(pico_rom)

target_sources(pico_rom PRIVATE rom.c)

target_include_directories(pico_rom PRIVATE
            ${CMAKE_CURRENT_LIST_DIR}
            ${CMAKE_CURRENT_LIST_DIR}/..
            )

target_link_libraries(pico_rom PRIVATE
        pico_stdlib
        hardware_pio
        hardware_adc
        pico_multicore
        )

target_compile_definitions(pico_rom PRIVATE
        
        )
# pico_set_binary_type(pico_rom copy_to_ram)

pico_add_extra_outputs(pico_rom)

# Enable USB output, disable UART output.
# pico_enable_stdio_usb(pico_rom 1)
# pico_enable_stdio_uart(pico_rom 0)
