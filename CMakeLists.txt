cmake_minimum_required(VERSION 3.16)
project(ImGui CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(IMGUI_SOURCES
    "imconfig.h"
    "imgui.h"
    "imgui.cpp"
    "imgui_draw.cpp"
    "imgui_internal.h"
    "imgui_widgets.cpp"
    "imstb_rectpack.h"
    "imgui_tables.cpp"
    "imstb_textedit.h"
    "imstb_truetype.h"
    "imgui_demo.cpp"
    
    "backends/imgui_impl_glfw.cpp"
    "backends/imgui_impl_vulkan.cpp"
)

add_library(ImGui STATIC ${IMGUI_SOURCES})

target_include_directories(ImGui PUBLIC 
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/backends 
)

# Find Vulkan
find_package(Vulkan REQUIRED)

target_link_libraries(ImGui PUBLIC 
    GLFW
    Vulkan::Vulkan
)

if(MSVC)
    set_property(TARGET ImGui PROPERTY 
        MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()