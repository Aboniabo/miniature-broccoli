cmake_minimum_required(VERSION 3.31)
project(GuiApp VERSION 0.1 LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

include(FetchContent)

# --- GLFW ---- #
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS    OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_DOCS     OFF CACHE BOOL "" FORCE)
set(GLFW_INSTALL        OFF CACHE BOOL "" FORCE)

FetchContent_declare(
        glfw
        GIT_REPOSITORY https://github.com/glfw/glfw.git
        GIT_TAG 3.4
)
FetchContent_MakeAvailable(glfw)

# --- ImGUI --- #
FetchContent_Declare(
        imgui
        GIT_REPOSITORY https://github.com/ocornut/imgui
        GIT_TAG docking
)
FetchContent_MakeAvailable(glfw)

# --- GLAD --- #
add_library(glad STATIC ${CMAKE_CURRENT_SOURCE_DIR}/vendor/glad/src/glad.c)
target_include_directories(glad PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/vendor/glad/include)
set_target_properties(glad PROPERTIES LINKER_LANGUAGE CXX)

# --- Build imgui static lib with GLFW and OPENGL3 --- #
set(IMGUI_DIR ${CMAKE_CURRENT_SOURCE_DIR}/vendor/imgui)
set( IMGUI_SOURCES
        ${IMGUI_DIR}/imgui.cpp
        ${IMGUI_DIR}/imgui_demo.cpp
        ${IMGUI_DIR}/imgui_draw.cpp
        ${IMGUI_DIR}/imgui_tables.cpp
        ${IMGUI_DIR}/imgui_widgets.cpp
        ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
        ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp
)

add_library(imgui STATIC ${IMGUI_SOURCES})
target_include_directories(imgui PUBLIC ${IMGUI_DIR} ${IMGUI_DIR}/backends)
target_compile_definitions(imgui PUBLIC IMGUI_IMPL_OPENGL_LOADER_GLAD)
find_package(OpenGL REQUIRED)
target_link_libraries(imgui PUBLIC glfw OpenGL::GL glad)

add_executable(GuiApp src/main.cpp)

target_include_directories(GuiApp PRIVATE src)
target_link_libraries(GuiApp PRIVATE imgui glfw glad OpenGL::GL)
