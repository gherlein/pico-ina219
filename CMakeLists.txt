cmake_minimum_required(VERSION 3.13)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

include(pico_sdk_import.cmake)

project(main C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
file(GLOB_RECURSE SOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "src/*.[ch]")


pico_sdk_init()

add_executable(main    
    ${SOURCES}
)

pico_enable_stdio_usb(main 0)
pico_enable_stdio_uart(main 1)

target_include_directories(main PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}
        include
        pico-i2clib/include
)
add_subdirectory(pico-i2clib)

pico_add_extra_outputs(main)

target_link_libraries(main
	pico_stdlib
	hardware_i2c 
	pico_multicore
    hardware_i2c
    hardware_gpio
    pico-i2clib
	# other libs as needed
)

add_custom_target(flash
    COMMAND /bin/bash ../flash
    DEPENDS main
)
add_custom_target(reset
    COMMAND /bin/bash ../reset
    DEPENDS main
)
