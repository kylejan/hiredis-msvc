cmake_minimum_required(VERSION 3.2)
project(hiredis)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/x64)
add_definitions(-D_WIN32_WINNT=0x0501)
add_definitions(-D_CRT_SECURE_NO_WARNINGS)
add_definitions(-D_UNICODE)

include_directories(${PROJECT_SOURCE_DIR}/include)

file(GLOB hiredis_headers ${PROJECT_SOURCE_DIR}/include/*.h)
file(GLOB win32_interop_headers ${PROJECT_SOURCE_DIR}/include/win32_interop/*.h)
file(GLOB hiredis_c_sources ${PROJECT_SOURCE_DIR}/src/*.c)
file(GLOB win32_interop_c_sources ${PROJECT_SOURCE_DIR}/src/win32_interop/*.c)
file(GLOB win32_interop_cpp_sources ${PROJECT_SOURCE_DIR}/src/win32_interop/*.cpp)

add_library(win32_interop ${win32_interop_headers} ${win32_interop_c_sources} ${win32_interop_cpp_sources})

add_library(hiredis ${hiredis_headers} ${hiredis_c_sources})

add_custom_command(TARGET win32_interop POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory ${PROJECT_SOURCE_DIR}/include/win32_interop ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}/include/win32_interop
)

add_custom_command(TARGET hiredis POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy ${PROJECT_SOURCE_DIR}/include/hiredis.h ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}/include/hiredis.h
)
add_custom_command(TARGET hiredis POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy ${PROJECT_SOURCE_DIR}/include/sds.h ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}/include/sds.h
)
add_custom_command(TARGET hiredis POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy ${PROJECT_SOURCE_DIR}/include/read.h ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}/include/read.h
)