cmake_minimum_required(VERSION 3.14)

if(NOT DEFINED VCPKG_ROOT)
    set(VCPKG_ROOT
        ${CMAKE_CURRENT_SOURCE_DIR}/vcpkg/
        CACHE PATH "Path to vcpkg directory"
    )
endif()

if(DEFINED VCPKG_ROOT)
  set(CMAKE_TOOLCHAIN_FILE
      "${VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake"
      CACHE PATH "Path to vcpkg toolchain")

  if(NOT EXISTS "${CMAKE_TOOLCHAIN_FILE}")
    message(
      FATAL_ERROR
        "Invalid VCPKG_ROOT specified (${VCPKG_ROOT}); please specify an absolute path"
    )
  endif()
endif()

project(cookiecutter-cpp-vcpkg CXX)

add_executable(cookiecutter-cpp-vcpkg src/main.cpp)

vcpkg_install_packages()

find_package(fmt CONFIG REQUIRED)
find_package(cppitertools CONFIG REQUIRED)

target_link_libraries(cookiecutter-cpp-vcpkg PRIVATE fmt::fmt cppitertools::cppitertools)
