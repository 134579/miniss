CPMAddPackage(
    NAME googletest
    GITHUB_REPOSITORY google/googletest
    GIT_TAG v1.18.0
    GIT_SHALLOW TRUE
    OPTIONS
        "INSTALL_GTEST OFF"
        "BUILD_GMOCK OFF"  # 测试只用 gtest，不构建 gmock
)

link_libraries(GTest::gtest GTest::gtest_main)
link_libraries(miniss)
include_directories(../include)

aux_source_directory(. CASES)
foreach(CASE ${CASES})
    cmake_path(GET CASE STEM FNAME)
    add_executable(${FNAME} ${CASE})
endforeach()
