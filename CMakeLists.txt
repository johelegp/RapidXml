cmake_minimum_required(VERSION 3.3.0)

project(RapidXml VERSION 5.0.0 LANGUAGES CXX)

add_library(RapidXml INTERFACE)
target_include_directories(RapidXml INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)
set_property(TARGET RapidXml APPEND PROPERTY
    COMPATIBLE_INTERFACE_STRING RapidXml_VERSION_MAJOR)

find_package(jegp 3.0.0 REQUIRED)
target_link_libraries(RapidXml INTERFACE jegp)

install(TARGETS RapidXml EXPORT RapidXmlTargets)
install(FILES
    rapidxml.hpp
    rapidxml_iterators.hpp
    rapidxml_print.hpp
    rapidxml_utils.hpp
    DESTINATION include)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(RapidXmlConfigVersion.cmake
    VERSION ${RapidXml_VERSION}
    COMPATIBILITY SameMajorVersion)

export(EXPORT RapidXmlTargets FILE RapidXmlTargets.cmake)
configure_file(RapidXmlConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/RapidXmlConfig.cmake
    COPYONLY)

install(EXPORT RapidXmlTargets DESTINATION cmake)
install(
    FILES
        ${CMAKE_CURRENT_BINARY_DIR}/RapidXmlConfig.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/RapidXmlConfigVersion.cmake
    DESTINATION cmake)
