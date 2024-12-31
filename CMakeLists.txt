cmake_minimum_required(VERSION 3.16)

project(doomenstein-3d)

include(FetchContent)

FetchContent_Declare(
    SDL2
    GIT_REPOSITORY https://github.com/libsdl-org/SDL.git
    GIT_TAG release-2.26.3
)
FetchContent_MakeAvailable(SDL2)

macro(add_exe name file)
add_executable(${name} ${file})
target_link_libraries(${name} PRIVATE SDL2::SDL2)

find_library(MATH_LIBRARY m)
if(MATH_LIBRARY)
    target_link_libraries(${name} PUBLIC ${MATH_LIBRARY})
endif()

if(WIN32)
    add_custom_command(
        TARGET doom POST_BUILD
        COMMAND "${CMAKE_COMMAND}" -E copy_if_different "$<TARGET_FILE:SDL2::SDL2>" "$<TARGET_FILE_DIR:${name}>"
        VERBATIM
    )
endif()
endmacro()

add_exe(doom src/main_doom.c)
add_exe(wolf src/main_wolf.c)
