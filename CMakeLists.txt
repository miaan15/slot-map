cmake_minimum_required(VERSION 3.15)

project(slot_map VERSION 0.1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

option(BUILD_TEST "Build tests" ON)

add_library(slot_map INTERFACE)

target_include_directories(slot_map INTERFACE src)
target_compile_features(slot_map INTERFACE cxx_std_17)

if(BUILD_TEST)
    include(FetchContent)
    message(STATUS "Fetching googletest...")
    FetchContent_Declare(
        googletest
        GIT_REPOSITORY https://github.com/google/googletest.git
        GIT_TAG v1.15.2
    )
    set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
    FetchContent_MakeAvailable(googletest)

    add_executable(slot_map_test test/slot_map_test.cpp)
    target_link_libraries(slot_map_test PRIVATE slot_map gtest_main)

    include(GoogleTest)
    gtest_discover_tests(slot_map_test)
endif()
