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
)

if (APPLE)
    list(APPEND IMGUI_SOURCES "backends/imgui_impl_metal.mm")
elseif (WIN32)
    list(APPEND IMGUI_SOURCES "backends/imgui_impl_opengl3.cpp")
endif()

add_library(ImGui STATIC ${IMGUI_SOURCES})

target_include_directories(ImGui PUBLIC 
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/backends 
)

target_link_libraries(ImGui PUBLIC GLFW)

if (APPLE)
    find_library(METAL_LIB Metal)
    find_library(QUARTZ_LIB QuartzCore)
    find_library(COCOA_LIB Cocoa)
    
    target_link_libraries(ImGui PRIVATE ${METAL_LIB} ${QUARTZ_LIB} ${COCOA_LIB})
    
    set_source_files_properties("backends/imgui_impl_metal.mm"
        PROPERTIES COMPILE_FLAGS "-fobjc-arc -Wno-nullability-completeness"
    )
endif()

if(MSVC)
    set_property(TARGET ImGui PROPERTY 
        MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()