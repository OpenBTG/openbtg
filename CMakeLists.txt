# Set minimum version to something recent
cmake_minimum_required(VERSION 3.13)
project(mortar)

find_package(OpenGL)
find_package(PkgConfig)

pkg_check_modules(GLFW3 REQUIRED glfw3)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/build)

# Build a static binary if this is Windows
# https://stackoverflow.com/questions/63178407/cmake-compile-in-linux-execute-in-windows
# https://stackoverflow.com/questions/68350419/generate-static-executable-with-cmake
if (${CMAKE_SYSTEM_NAME} MATCHES "Windows")
        set(BUILD_FOR_WIN TRUE)
endif()
option(STATIC_BUILD "Build a static binary." ${BUILD_FOR_WIN})
if (STATIC_BUILD)
        set(CMAKE_EXE_LINKER_FLAGS "-static")
        set(CMAKE_FIND_LIBRARY_SUFFIXES ".a" CONFIG)
        set(BUILD_SHARED_LIBS OFF)
        set_target_properties(main-static PROPERTIES
                LINK_SEARCH_START_STATIC ON
                LINK_SEARCH_END_STATIC ON
        )
)
endif()

set(SRCS
        main.cpp
        dds.cpp
        filestream.cpp
        glmodel.cpp
        hgp.cpp
        lsw.cpp
        matrix.cpp
        memorystream.cpp
        model.cpp
        nup.cpp
        stream.cpp
        texture.cpp
)

add_executable(mortar ${SRCS})
target_link_libraries(mortar ${OPENGL_LIBRARIES} ${GLFW3_LIBRARIES} ${GLFW3_STATIC_LIBRARIES})
