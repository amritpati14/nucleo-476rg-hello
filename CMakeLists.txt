cmake_minimum_required(VERSION 3.7)
project(arm_hello_world ASM C)

set(CMAKE_EXECUTABLE_SUFFIX ".elf")

# set up compiler specific options
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
# CPU architecture specific flags
set(CPU_FLAGS "-mthumb -mcpu=cortex-m4")
set(GCC_FLAGS "-ffunction-sections -fdata-sections -fmessage-length=0 -fsigned-char -fshort-enums")
set(STM32L4_LD_SCRIPT "${CMAKE_SOURCE_DIR}/hal/system/armgcc/STM32L476RGTx_FLASH.ld")
set(CMAKE_EXE_LINKER_FLAGS "--specs=nano.specs -Xlinker --gc-sections")

set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${CPU_FLAGS} -T\"${STM32L4_LD_SCRIPT}\"")

# CPU defs
add_definitions(${CPU_FLAGS} ${GCC_FLAGS} --specs=nano.specs)
# System defs
add_definitions(-DSTM32L476xx -DUSE_HAL_DRIVER)

set(CMSIS_DIR ${CMAKE_SOURCE_DIR}/hal/cmsis)
set(STM32L4XX_HAL ${CMAKE_SOURCE_DIR}/hal/stm32l4xx_hal/inc)
set(STM32L4XX_HAL_LEGACY ${STM32L4XX_HAL}/legacy)
set(SYSTEM_DIR ${CMAKE_SOURCE_DIR}/hal/system)
set(APP_DIR ${CMAKE_SOURCE_DIR}/app)
set(BSP_DIR ${CMAKE_SOURCE_DIR}/hal/bsp)

include_directories(
  ${CMSIS_DIR}
  ${STM32L4XX_HAL}
  ${STM32L4XX_HAL_LEGACY}
  ${SYSTEM_DIR}
  ${APP_DIR}
  ${BSP_DIR}
)

add_subdirectory(app/)
add_subdirectory(hal/)
