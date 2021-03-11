add_executable(cookiecutter-cpp-vcpkg main.cpp)

find_package(fmt CONFIG REQUIRED)
find_package(cppitertools CONFIG REQUIRED)
target_compile_features(cookiecutter-cpp-vcpkg PRIVATE cxx_std_17)
target_include_directories(cookiecutter-cpp-vcpkg PRIVATE ${PROJECT_SOURCE_DIR}/include)
target_link_libraries(cookiecutter-cpp-vcpkg PRIVATE fmt::fmt)
