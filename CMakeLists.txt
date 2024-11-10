# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)

# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.0.0)
set(toolchainVersion 13_2_Rel1)
set(picotoolVersion 2.0.0)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
set(PICO_BOARD pico CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

project(TestSDCard C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1

add_executable(TestSDCard TestSDCard.c hw_config.c ) #ADD CUSTOM HW CONFIG FOR OUR SD CARD SETUP

pico_set_program_name(TestSDCard "TestSDCard")
pico_set_program_version(TestSDCard "0.1")

# *************************************Tell CMake where to find other source code SD CARD
add_subdirectory(lib/no-OS-FatFS-SD-SDIO-SPI-RPi-Pico/src build)

# Modify the below lines to enable/disable output over UART/USB
pico_enable_stdio_uart(TestSDCard 1)
pico_enable_stdio_usb(TestSDCard 0)

# Add the standard library to the build
target_link_libraries(TestSDCard
        pico_stdlib
        no-OS-FatFS-SD-SDIO-SPI-RPi-Pico)
        # ***********************************NEED TO ADD LINE ABOVE ********************

# Add the standard include files to the build
target_include_directories(TestSDCard PRIVATE
  ${CMAKE_CURRENT_LIST_DIR}
)

# Add any user requested libraries
target_link_libraries(TestSDCard 
        hardware_spi
        hardware_i2c
        )

pico_add_extra_outputs(TestSDCard)

