cmake_minimum_required(VERSION 3.23)

set(CMAKE_CXX_STANDARD 20)

option(IMGUI_BACKEND_METAL "Use the Metal backend" OFF)
option(IMGUI_BACKEND_OSX "Use the MacOS backend" OFF)
option(IMGUI_BACKEND_GLFW "Use the GLFW backend" OFF)

project(
  imgui
  VERSION 1.90.4
  DESCRIPTION ""
  LANGUAGES CXX)

add_library(ImGui STATIC)

target_include_directories(ImGui
  PUBLIC
    ${PROJECT_SOURCE_DIR}
)

target_sources(ImGui
  PRIVATE
    imconfig.h
    imgui.cpp
    imgui.h
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_internal.h
    imgui_tables.cpp
    imgui_widgets.cpp
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
)

function(add_backend option_name)
  if(NOT option_name)
    return()
  endif()
  cmake_parse_arguments(
    PARSE_ARGV 1 ARG ";" ";" "SOURCES_VALUE;SOURCES")
  #message("${ARG_SOURCES}")
  target_sources(ImGui
    PRIVATE
      ${ARG_SOURCES}
  )
endfunction()

add_backend(${IMGUI_BACKEND_METAL}
  SOURCES
    backends/imgui_impl_metal.h
    backends/imgui_impl_metal.mm
)

add_backend(${IMGUI_BACKEND_OSX}
  SOURCES
    backends/imgui_impl_osx.h
    backends/imgui_impl_osx.mm
)

add_backend(${IMGUI_BACKEND_GLFW}
  SOURCES
    backends/imgui_impl_glfw.h
    backends/imgui_impl_glfw.cpp
)
