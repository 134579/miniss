cmake_minimum_required(VERSION 3.22)
project(miniss)

set(CMAKE_CXX_STANDARD 20)

aux_source_directory(lib SRC)
aux_source_directory(lib/io SRC1)
aux_source_directory(lib/poller SRC2)
aux_source_directory(lib/util SRC3)

add_library(${PROJECT_NAME} ${SRC} ${SRC1} ${SRC2} ${SRC3})
target_include_directories(${PROJECT_NAME} PRIVATE include)

include(${CMAKE_CURRENT_SOURCE_DIR}/cmake/get_cpm.cmake)

CPMAddPackage(
    NAME fmt
    GITHUB_REPOSITORY fmtlib/fmt
    # 9.1.0 的 consteval 格式串检查在新版 clang 下报错（fmt 10 起重写），需 >= 10
    GIT_TAG 11.2.0
    GIT_SHALLOW TRUE
    OPTIONS "FMT_INSTALL OFF"
)

CPMAddPackage(
    NAME spdlog
    GITHUB_REPOSITORY gabime/spdlog
    # 与上面的 fmt 配套（spdlog 1.15.3 自带的正是 fmt 11.2.0）；1.11.0 不支持 fmt 10+
    GIT_TAG v1.15.3
    GIT_SHALLOW TRUE
    OPTIONS
        "SPDLOG_INSTALL OFF"
        "SPDLOG_FMT_EXTERNAL ON"
        "SPDLOG_FMT_EXTERNAL_HO OFF"
)

target_link_libraries(${PROJECT_NAME} PUBLIC fmt::fmt spdlog::spdlog rt)

# include(CTest) 会调用 enable_testing() 并额外生成 DartConfiguration.tcl
# （VS Code 的测试面板需要它，用 enable_testing() 会一直报 "Cannot find file"）
include(CTest)
if (BUILD_TESTING)
    add_subdirectory(tests)
endif ()