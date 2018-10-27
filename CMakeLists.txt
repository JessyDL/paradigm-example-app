cmake_minimum_required(VERSION 3.11)
include(FetchContent)
set(PROJECT_EXAMPLE_GAME example_game CACHE INTERNAL "")
set(LOCAL_PROJECT ${PROJECT_EXAMPLE_GAME})
project(${LOCAL_PROJECT} VERSION 1.0.0 LANGUAGES CXX)

FetchContent_Declare(
  paradigm
  GIT_REPOSITORY https://github.com/JessyDL/paradigm
  GIT_TAG        develop
)

FetchContent_GetProperties(paradigm)
if(NOT paradigm_POPULATED)
  FetchContent_Populate(paradigm)
  add_subdirectory(${paradigm_SOURCE_DIR} ${paradigm_BINARY_DIR})
endif()

set(WSI_LIB ${PARADIGM_WSI_LIB})
set(CMAKE_CXX_FLAGS ${PARADIGM_CMAKE_CXX_FLAGS})
set(CMAKE_CXX_FLAGS_RELEASE ${PARADIGM_CMAKE_CXX_FLAGS_RELEASE})
set(CMAKE_CXX_FLAGS_DEBUG ${PARADIGM_CMAKE_CXX_FLAGS_DEBUG})
set(CMAKE_EXE_LINKER_FLAGS ${PARADIGM_CMAKE_EXE_LINKER_FLAGS})
set(CMAKE_SHARED_LINKER_FLAGS ${PARADIGM_CMAKE_SHARED_LINKER_FLAGS})
set(COMPILE_OPTIONS ${PARADIGM_COMPILE_OPTIONS})
set(COMPILE_OPTIONS_EXE ${PARADIGM_COMPILE_OPTIONS_EXE})
set(COMPILE_OPTIONS_LIB  ${PARADIGM_COMPILE_OPTIONS_LIB})

add_definitions(${PARADIGM_DEFINES})	

setup_vulkan("${VULKAN_ROOT}" "${VULKAN_VERSION}" VULKAN_INC_PATH VULKAN_LIB_PATH VULKAN_LIB_NAME)

include(inc.txt)
include(src.txt)

source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}/inc" PREFIX "inc" FILES ${INC}) 
source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}/src" PREFIX "src" FILES ${SRC}) 

add_executable(${LOCAL_PROJECT} ${SRC} ${INC})
set(_PCH_SOURCE_CXX "src/stdafx.cpp")
add_precompiled_header(${LOCAL_PROJECT} "stdafx.h")

target_link_libraries(${LOCAL_PROJECT} PUBLIC ${PROJECT_CORE} )

target_include_directories(${LOCAL_PROJECT}
    PUBLIC 
        ${CMAKE_CURRENT_SOURCE_DIR}/inc
)

target_compile_features(${LOCAL_PROJECT} PUBLIC ${PROJECT_COMPILER_FEATURES} PRIVATE ${PROJECT_COMPILER_FEATURES_PRIVATE})
target_compile_options(${LOCAL_PROJECT} PRIVATE ${COMPILE_OPTIONS} ${COMPILE_OPTIONS_EXE})