cmake_minimum_required(VERSION 3.3)
project(lottie2gif)

set(CMAKE_CXX_STANDARD 17)

# 引入rlottie
add_subdirectory(rlottie)
include_directories(rlottie/inc)

# 指定可执行文件存放目录
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/output)

add_executable(lottie2gif
        main.cpp
        gif.h
)
target_link_libraries(lottie2gif PRIVATE rlottie)

# 按照平台不同，转移动态库
IF (CMAKE_SYSTEM_NAME MATCHES "Linux")

ELSEIF (CMAKE_SYSTEM_NAME MATCHES "Windows")
    add_custom_command(
            TARGET lottie2gif POST_BUILD
            COMMAND ${CMAKE_COMMAND} -E copy_if_different
            "${CMAKE_BINARY_DIR}/rlottie/librlottie.dll"
            ${EXECUTABLE_OUTPUT_PATH}
    )
ELSEIF (CMAKE_SYSTEM_NAME MATCHES "FreeBSD")

ELSE ()
    MESSAGE(STATUS "other platform: ${CMAKE_SYSTEM_NAME}")
ENDIF (CMAKE_SYSTEM_NAME MATCHES "Linux")


