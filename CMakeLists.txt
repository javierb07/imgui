cmake_minimum_required(VERSION 3.10)
project(ImGui)

# ImGui core source files
set(IMGUI_SOURCES
    imgui.cpp
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
)

# ImGui header files
set(IMGUI_HEADERS
    imconfig.h
    imgui.h
    imgui_internal.h
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
)

# Backend files - add the ones you need
# For this project, we'll include Win32 and OpenGL3 backends
set(IMGUI_BACKEND_SOURCES
    backends/imgui_impl_win32.cpp
    backends/imgui_impl_opengl3.cpp
)

set(IMGUI_BACKEND_HEADERS
    backends/imgui_impl_win32.h
    backends/imgui_impl_opengl3.h
    backends/imgui_impl_opengl3_loader.h
)

# Create static library
add_library(imgui STATIC
    ${IMGUI_SOURCES}
    ${IMGUI_HEADERS}
    ${IMGUI_BACKEND_SOURCES}
    ${IMGUI_BACKEND_HEADERS}
)

# Include directories
target_include_directories(imgui
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/backends
)

# Set compile definitions
target_compile_definitions(imgui
    PUBLIC
        IMGUI_IMPL_OPENGL_LOADER_GLAD
)

# Set compiler options
if(MSVC)
    target_compile_options(imgui PRIVATE /W4)
else()
    target_compile_options(imgui PRIVATE -Wall -Wextra -Wpedantic)
endif()

# Find OpenGL (needed for OpenGL3 backend)
find_package(OpenGL REQUIRED)
target_link_libraries(imgui PUBLIC ${OPENGL_LIBRARIES})

# Export imgui targets
set_target_properties(imgui PROPERTIES
    FOLDER "Dependencies"
    POSITION_INDEPENDENT_CODE ON
)
