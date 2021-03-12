cmake_minimum_required(VERSION 3.14)

if(NOT DEFINED VCPKG_ROOT)
  set(VCPKG_ROOT
      ${CMAKE_CURRENT_SOURCE_DIR}/vcpkg
      CACHE PATH "Path to vcpkg directory")
endif()

if(DEFINED VCPKG_ROOT)
  set(CMAKE_TOOLCHAIN_FILE
      "${VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake"
      CACHE PATH "Path to vcpkg toolchain")
  if(NOT EXISTS "${CMAKE_TOOLCHAIN_FILE}")
    message(
      FATAL_ERROR
        "Invalid CMAKE_TOOLCHAIN_FILE specified (${CMAKE_TOOLCHAIN_FILE}, ${VCPKG_ROOT}); please specify an absolute path"
    )
  endif()
endif()

set(APP_LOW_NAME "cookiecutter-cpp-vcpkg")
set(APP_DISPLAY_NAME "Cookiecutter Cpp Vcpkg")
set(APP_VERSION "0.0.1")
set(APP_URL "https://github.com/robertobernabe/cookiecutter-cpp-vcpkg")
set(APP_EMAIL "hello@fschaeffeler.de")
set(APP_COMPANY "Florian Schäffeler")

project(cookiecutter-cpp-vcpkg CXX)
option(USE_NSIS "Use NSIS generator to produce installer" OFF)

add_subdirectory(src)
include(CTest)
add_subdirectory(test)

if(USE_NSIS)
  set(CPACK_GENERATOR "NSIS")
else(USE_NSIS)
  set(CPACK_GENERATOR "ZIP")
endif(USE_NSIS)

set(CPACK_PACKAGE_NAME ${APP_LOW_NAME})
set(CPACK_PACKAGE_VERSION ${APP_VERSION})
set(CPACK_PACKAGE_VERSION_PATCH "0")
set(CPACK_PACKAGE_VENDOR ${APP_COMPANY})
set(CPACK_PACKAGE_INSTALL_REGISTRY_KEY ${APP_NAME})
set(CPACK_IGNORE_FILES
    "\\.psd$;/CVS/;/\\.svn/;/\\.git/;\\.swp$;/CMakeLists.txt.user;\\.#;/#;\\.tar.gz$;/CMakeFiles/;CMakeCache.txt;\\.qm$;/build/;\\.diff$;.DS_Store'"
)
set(CPACK_SOURCE_GENERATOR "ZIP")
set(CPACK_SOURCE_PACKAGE_FILE_NAME
    "${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}")
set(CPACK_SOURCE_IGNORE_FILES ${CPACK_IGNORE_FILES})

set(CPACK_NSIS_CONTACT ${APP_EMAIL})
set(CPACK_NSIS_DISPLAY_NAME ${APP_DISPLAY_NAME})
set(CPACK_NSIS_HELP_LINK ${APP_URL})
set(CPACK_NSIS_INSTALLED_ICON_NAME
    "${PROJECT_SOURCE_DIR}/resources/${APP_LOW_NAME}.ico")
set(CPACK_NSIS_MUI_ICON "${PROJECT_SOURCE_DIR}/resources/${APP_LOW_NAME}.ico")
set(CPACK_NSIS_MUI_UNIICON
    "${PROJECT_SOURCE_DIR}/resources/${APP_LOW_NAME}.ico")
set(CPACK_NSIS_PACKAGE_NAME ${APP_DISPLAY_NAME})
set(CPACK_NSIS_URL_INFO_ABOUT ${APP_URL})

include(CPack)
install(TARGETS cookiecutter-cpp-vcpkg RUNTIME DESTINATION bin
                                               COMPONENT applications)
