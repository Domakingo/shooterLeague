cmake_minimum_required(VERSION 3.14)
project(shooterLeague)

set(EXEC_NAME shooterLeague)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
option(BUILD_SHARED_LIBS "Build shared libraries" OFF)

include(FetchContent)
FetchContent_Declare(
        SFML
        GIT_REPOSITORY https://github.com/SFML/SFML.git
        GIT_TAG 2.6.x
)
FetchContent_MakeAvailable(SFML)

set(INCLUDE_DIR
        ${CMAKE_SOURCE_DIR}/source/headers
        ${CMAKE_SOURCE_DIR}/source/assets
)

set(SOURCE_FILES
        source/src/main.cpp
        source/src/engine.cpp
        source/src/draw.cpp
        source/src/input.cpp
        source/src/update.cpp
        source/src/car.cpp
)

include_directories(${INCLUDE_DIR})

add_executable(${EXEC_NAME} ${SOURCE_FILES})

target_link_libraries(shooterLeague PRIVATE sfml-graphics)
target_compile_features(shooterLeague PRIVATE cxx_std_20)

if(WIN32)
    add_custom_command(
            TARGET shooterLeague
            COMMENT "Copy OpenAL DLL"
            PRE_BUILD COMMAND ${CMAKE_COMMAND} -E copy
            ${SFML_SOURCE_DIR}/extlibs/bin/$<IF:$<EQUAL:${CMAKE_SIZEOF_VOID_P},8>,x64,x86>/openal32.dll
            $<TARGET_FILE_DIR:shooterLeague>
            VERBATIM
    )
endif()
