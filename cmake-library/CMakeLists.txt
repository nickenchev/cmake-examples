cmake_minimum_required(VERSION 3.28)
project(LibraryDemo LANGUAGES CXX)

add_library(VeryUseful
    "src/useful.cpp"
)

set_target_properties(VeryUseful
    PROPERTIES
    CMAKE_CXX_STANDARD 20
    CMAKE_CXX_STANDARD_REQUIRED ON
    CMAKE_CXX_EXTENSIONS OFF
)

# cache variables for installation destinations
include(GNUInstallDirs)

target_include_directories(VeryUseful
    PUBLIC
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>"
    "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>"
)

install(TARGETS VeryUseful
    EXPORT VeryUsefulTargets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

install(FILES src/useful.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(EXPORT VeryUsefulTargets
    FILE VeryUsefulTargets.cmake
    NAMESPACE VeryUseful::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/VeryUseful
)

# include the package config helpers and generate the file below
include(CMakePackageConfigHelpers)

configure_package_config_file(${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in
  "${CMAKE_CURRENT_BINARY_DIR}/VeryUsefulConfig.cmake"
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/VeryUseful
)

install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/VeryUsefulConfig.cmake"
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/VeryUseful
)
