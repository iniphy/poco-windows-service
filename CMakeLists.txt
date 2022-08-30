cmake_minimum_required(VERSION 3.0)

project(poco-windows-service VERSION "0.1.0")

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_BUILD_TYPE Release)
set(CMAKE_CXX_FLAGS_RELEASE "/O2")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin/)

# From: https://stackoverflow.com/a/60844772/1319478
if (MSVC)
    add_definitions(-D_WIN32_WINNT=0x600)
    add_compile_options(
            $<$<CONFIG:>:/MT> #---------|
            $<$<CONFIG:Debug>:/MTd> #---|-- Statically link the runtime libraries
            $<$<CONFIG:Release>:/MT> #--|
    )
endif ()

set(POCO_PATH third-party/poco)

set(BUILD_SHARED_LIBS OFF CACHE BOOL "Build POCO as static libraries!" FORCE)
set(POCO_MT ON CACHE BOOL "Build POCO as static libraries!" FORCE)
add_subdirectory(${POCO_PATH})

add_executable(service-app main.cpp App.hpp App.cpp)

target_link_libraries(service-app Poco::Util)
