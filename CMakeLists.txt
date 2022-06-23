
cmake_minimum_required (VERSION 3.13...3.21)

if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
    set(CMAKE_TOOLCHAIN_FILE
        "${CMAKE_CURRENT_SOURCE_DIR}/vcpkg/scripts/buildsystems/vcpkg.cmake"
        CACHE STRING "Vcpkg toolchain file")
endif()

project(
    voidshade
    LANGUAGES C CXX
)

set(
    SOURCE_FILES
    ${PROJECT_SOURCE_DIR}/voidshade/test.cpp
)

add_library(${PROJECT_NAME} ${SOURCE_FILES})

target_include_directories(${PROJECT_NAME} PUBLIC "${PROJECT_SOURCE_DIR}")

# Ensure the C++17 standard is available.
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_17)

add_subdirectory(libtcod)  # This will error if you haven't initialized libtcod the submodule!
find_package(SDL2 CONFIG REQUIRED)

target_link_libraries(${PROJECT_NAME} PRIVATE
    SDL2::SDL2
    SDL2::SDL2main
    libtcod::libtcod
)


# Enforce UTF-8 encoding on MSVC.
if (MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE /utf-8)
endif()
