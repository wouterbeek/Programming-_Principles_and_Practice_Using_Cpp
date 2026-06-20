cmake_minimum_required(VERSION 4.3.2)
project(ppp-cpp
  DESCRIPTION "A very simple boilerplate for cross-OS C++ compilation with dependency management."
  HOMEPAGE_URL https://github.com/wouterbeek/cpp-boilerplate
  LANGUAGES CXX
  VERSION 0.1.0
)

set(cppVersion 23)
set(CXX_EXTENSIONS OFF)
set(CXX_STANDARD ${cppVersion})
set(CXX_STANDARD_REQUIRED ON)

add_library(base INTERFACE)
add_library(${PROJECT_NAME}::base ALIAS base)
target_compile_features(base INTERFACE cxx_std_${cppVersion})

find_package(fltk REQUIRED)

add_library(ppp STATIC)
add_library(${PROJECT_NAME}::ppp ALIAS ppp)
target_include_directories(ppp PUBLIC ${CMAKE_SOURCE_DIR})
target_link_libraries(ppp
  PUBLIC
    fltk::fltk
    ${PROJECT_NAME}::base
)
target_sources(ppp
  PRIVATE
    Graph.cpp
    GUI.cpp
    Simple_window.cpp
    Window.cpp
)

add_executable(main)
target_link_libraries(main
  PRIVATE
    ${PROJECT_NAME}::ppp
)
target_sources(main
  PRIVATE
    tool/main.cpp
)
