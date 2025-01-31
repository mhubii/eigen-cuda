cmake_minimum_required(VERSION 3.14)

set(CMAKE_DISABLE_SOURCE_CHANGES  ON)
set(CMAKE_DISABLE_IN_SOURCE_BUILD ON)

project(eigen-cuda LANGUAGES CXX)

option(USE_CUDA "Whether to use CUDA" OFF)

if (USE_CUDA)
    enable_language(CUDA)
endif ()

include(FetchContent)
FetchContent_Declare(
    Eigen3
    GIT_REPOSITORY git@gitlab.com:libeigen/eigen.git
    GIT_TAG master
    )
    
    FetchContent_MakeAvailable(Eigen3)
    
if (USE_CUDA)
    add_library(test
        ${CMAKE_CURRENT_SOURCE_DIR}/src/kernel.cu
        ${CMAKE_CURRENT_SOURCE_DIR}/src/test.cpp
    )
    target_compile_definitions(test PRIVATE
        USE_CUDA
    )
    set_property(TARGET test
        PROPERTY CUDA_SEPARABLE_COMPILATION ON 
    )
else ()
    add_library(test
        ${CMAKE_CURRENT_SOURCE_DIR}/src/kernel.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/src/test.cpp
    )
endif ()

target_include_directories(test
    PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include
)
target_link_libraries(test Eigen3::Eigen)

add_executable(main main.cpp)
target_link_libraries(main test)
