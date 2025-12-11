cmake_minimum_required(VERSION 3.16)
project(ImGui CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(ImGui STATIC
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
)

# This allows Crimson to find imgui.h just by linking to this target
target_include_directories(ImGui PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# Static Runtime (staticruntime "on")
# Forces /MT (Release) and /MTd (Debug) on MSVC
if(MSVC)
    set_property(TARGET ImGui PROPERTY 
        MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()