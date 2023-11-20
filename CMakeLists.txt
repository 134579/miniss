cmake_minimum_required(VERSION 3.22)
project(miniss)

set(CMAKE_CXX_STANDARD 20)
add_definitions(-D_GLIBCXX_USE_CXX11_ABI=0)


aux_source_directory(lib SRC)
aux_source_directory(lib/io SRC1)
aux_source_directory(lib/poller SRC2)
aux_source_directory(lib/util SRC3)

add_library(${PROJECT_NAME} ${SRC} ${SRC1} ${SRC2} ${SRC3})
target_include_directories(${PROJECT_NAME} PRIVATE include)

find_package(fmt CONFIG REQUIRED)
find_package(spdlog CONFIG REQUIRED)

target_link_libraries(${PROJECT_NAME} PRIVATE fmt::fmt spdlog::spdlog)

enable_testing()
add_subdirectory(tests)