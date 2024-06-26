cmake_minimum_required(VERSION 3.26)

project(sdl2_app C CXX)

# set where the compiled files goes
set(CMAKE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/libs)
set(CMAKE_PDB_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
LINK_DIRECTORIES(${CMAKE_BINARY_DIR}/libs)

file(
    DOWNLOAD
    https://github.com/cpm-cmake/CPM.cmake/releases/download/v0.39.0/CPM.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/cmake/CPM.cmake
    EXPECTED_HASH SHA256=66639bcac9dd2907b2918de466783554c1334446b9874e90d38e3778d404c2ef
)
include(${CMAKE_CURRENT_BINARY_DIR}/cmake/CPM.cmake)

#file(LOCK ${CPM_SOURCE_CACHE} DIRECTORY GUARD FILE)
string(TIMESTAMP BEFORE "%s")
CPMAddPackage("gh:libsdl-org/SDL#release-2.30.3")
CPMAddPackage("gh:libsdl-org/SDL_image#release-2.8.2")
CPMAddPackage("gh:libsdl-org/SDL_ttf#release-2.22.0")
CPMAddPackage("gh:libsdl-org/SDL_mixer#release-2.8.0")
CPMAddPackage("gh:libsdl-org/SDL_net#release-2.2.0")
#find_package(OpenGL REQUIRED)
# add libraries
#include(external/imgui.cmake)

# CPMAddPackage(
#   NAME IMGUI
#   GITHUB_REPOSITORY ocornut/imgui
#   GIT_TAG v1.90.6
#   DOWNLOAD_ONLY True
# )
#file(LOCK ${CPM_SOURCE_CACHE} DIRECTORY GUARD FILE RELEASE)

# MESSAGE(STATUS "IMGUI ===========================================================")
# MESSAGE(STATUS "IMGUI_ADDED: ${IMGUI_ADDED}")
# MESSAGE(STATUS "IMGUI ===========================================================")

IF(IMGUI_ADDED)
  # MESSAGE(STATUS "IMGUI ===========================================================")
  # MESSAGE(STATUS "DIR: ${IMGUI_SOURCE_DIR}")
  # MESSAGE(STATUS "IMGUI ===========================================================")
  #add_library(IMGUI)
  set(IMGUI_SOURCE_FILES
    ${IMGUI_SOURCE_DIR}/imgui.cpp
    ${IMGUI_SOURCE_DIR}/imgui_demo.cpp
    ${IMGUI_SOURCE_DIR}/imgui_draw.cpp
    ${IMGUI_SOURCE_DIR}/imgui_tables.cpp
    ${IMGUI_SOURCE_DIR}/imgui_widgets.cpp
    ${IMGUI_SOURCE_DIR}/backends/imgui_impl_sdl2.cpp
    ${IMGUI_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp
    ${IMGUI_SOURCE_DIR}/backends/imgui_impl_sdlrenderer2.cpp
  )
  set(IMGUI_INCLUDES
    ${IMGUI_SOURCE_DIR}
    ${IMGUI_SOURCE_DIR}/backends
  )
  include_directories(${IMGUI_INCLUDES})
ENDIF()

CPMAddPackage("gh:cimgui/cimgui#67cf8c96b77e6729e720a376757964ff2d2e78c0")
# MESSAGE(STATUS "IMGUI ===========================================================")
# MESSAGE(STATUS "cimgui_ADDED: ${cimgui_ADDED}")
# MESSAGE(STATUS "cimgui_INCLUDES: ${cimgui_INCLUDES}")
# MESSAGE(STATUS "cimgui_INCLUDES: ${cimgui_SOURCE_DIR}")
# MESSAGE(STATUS "IMGUI ===========================================================")
#add include for 
IF(cimgui_ADDED)
  include_directories(
    ${cimgui_SOURCE_DIR}/generator/output
    ${cimgui_SOURCE_DIR}
    ${cimgui_INCLUDES}
  )
  set(cimgui_SOURCE_FILES
    ${cimgui_SOURCE_DIR}/cimgui.cpp
    ${cimgui_SOURCE_DIR}/imgui/imgui.cpp
    ${cimgui_SOURCE_DIR}/imgui/imgui_tables.cpp
    ${cimgui_SOURCE_DIR}/imgui/imgui_draw.cpp
    ${cimgui_SOURCE_DIR}/imgui/imgui_demo.cpp
    ${cimgui_SOURCE_DIR}/imgui/imgui_widgets.cpp
    ${cimgui_SOURCE_DIR}/imgui/backends/imgui_impl_sdl2.cpp
    ${cimgui_SOURCE_DIR}/imgui/backends/imgui_impl_opengl3.cpp
  )

  set(cimgui_INCLUDES
    ${IMGUI_SOURCE_DIR}/imgui/
    ${IMGUI_SOURCE_DIR}/imgui/backends
  )

  include_directories(${cimgui_INCLUDES})
ENDIF()

# https://stackoverflow.com/questions/54864191/link-sdl2-net-with-cmake
# target_link_libraries(${PROJECT_NAME} SDL2_net::SDL2_net)
add_library(${PROJECT_NAME} INTERFACE)
target_link_libraries(${PROJECT_NAME} INTERFACE
#Vulkan::Headers
#SPIRV-Headers::SPIRV-Headers
#volk::volk_headers
#glslang
#GPUOpen::VulkanMemoryAllocator
SDL2::SDL2
SDL2_image::SDL2_image
SDL2_mixer::SDL2_mixer
SDL2_ttf::SDL2_ttf
opengl32
#cglm
#assimp::assimp
SDL2_net::SDL2_net
cimgui
#enet
)

if (PROJECT_IS_TOP_LEVEL)
    #add_executable(${PROJECT_NAME}_test test/main.c test/deps.cpp)
    #add_executable(${PROJECT_NAME}_test src/test.c)
    #add_executable(${PROJECT_NAME}_test src/sdl2_window.cpp)
    #add_executable(${PROJECT_NAME}_test src/sdl2_opengl_imgui.cpp ${IMGUI_SOURCE_FILES})
    #add_executable(${PROJECT_NAME}_test src/sdl2_renderer_imgui.cpp ${IMGUI_SOURCE_FILES})
    add_executable(${PROJECT_NAME}_test src/sdl2_opengl_imgui.c ${cimgui_SOURCE_FILES})
    target_link_libraries(${PROJECT_NAME}_test PRIVATE ${PROJECT_NAME})
endif ()

string(TIMESTAMP AFTER "%s")
math(EXPR DELTA_BUILD "${AFTER} - ${BEFORE}")
MESSAGE(STATUS "[[=== BUILD ===]]")
MESSAGE(STATUS "SDL2 APP BUILD TIME: ${DELTA_BUILD}s")
MESSAGE(STATUS "[[=== BUILD ===]]")

