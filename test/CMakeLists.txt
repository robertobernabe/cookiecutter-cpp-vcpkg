enable_testing()

find_package(GTest CONFIG REQUIRED)
include(GoogleTest)

add_executable(
  test_main
  test_main.cpp)

target_include_directories(test_main PRIVATE ${PROJECT_SOURCE_DIR}/include)
# I'm using C++17 in the test
target_compile_features(test_main PRIVATE cxx_std_17)
target_link_libraries(test_main PRIVATE GTest::gtest GTest::gtest_main)

# gtest_add_tests(TARGET test_main)
gtest_discover_tests(test_main)
