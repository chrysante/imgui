cmake_minimum_required(VERSION 3.23)

set(CMAKE_CXX_STANDARD 20)

option(IMGUI_BACKEND_METAL "Use the Metal backend" OFF)
option(IMGUI_BACKEND_OSX "Use the MacOS backend" OFF)
option(IMGUI_BACKEND_GLFW "Use the GLFW backend" OFF)
option(IMGUI_TEST_ENGINE "Compile the test engine" OFF)

project(
  imgui
  VERSION 1.90.4
  DESCRIPTION ""
  LANGUAGES CXX)

add_library(imgui STATIC)

target_include_directories(imgui
  PUBLIC
    ${PROJECT_SOURCE_DIR}
)

target_sources(imgui
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
  target_sources(imgui
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

if(NOT ${IMGUI_TEST_ENGINE})
  return()
endif()

message("Test Engine enabled")

target_include_directories(imgui
  PUBLIC
    ${PROJECT_SOURCE_DIR}/imgui_test_engine
)

target_sources(imgui
  PRIVATE
    imgui_test_engine/imgui_capture_tool.cpp
    imgui_test_engine/imgui_capture_tool.h
    imgui_test_engine/imgui_te_context.cpp
    imgui_test_engine/imgui_te_context.h
    imgui_test_engine/imgui_te_coroutine.cpp
    imgui_test_engine/imgui_te_coroutine.h
    imgui_test_engine/imgui_te_engine.cpp
    imgui_test_engine/imgui_te_engine.h
    imgui_test_engine/imgui_te_exporters.cpp
    imgui_test_engine/imgui_te_exporters.h
    imgui_test_engine/imgui_te_imconfig.h
    imgui_test_engine/imgui_te_internal.h
    imgui_test_engine/imgui_te_perftool.cpp
    imgui_test_engine/imgui_te_perftool.h
    imgui_test_engine/imgui_te_ui.cpp
    imgui_test_engine/imgui_te_ui.h
    imgui_test_engine/imgui_te_utils.cpp
    imgui_test_engine/imgui_te_utils.h
    imgui_test_engine/thirdparty/stb/imstb_image_write.h
    imgui_test_engine/thirdparty/Str/Str.h
)

target_compile_definitions(imgui PUBLIC
  IMGUI_ENABLE_TEST_ENGINE
  IMGUI_TEST_ENGINE_ENABLE_COROUTINE_STDTHREAD_IMPL=1)
