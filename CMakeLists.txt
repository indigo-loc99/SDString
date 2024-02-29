cmake_minimum_required(VERSION 3.23)

project(SDString 
	  VERSION 0.0.1 
	  DESCRIPTION "Standard library compatible secure-deleting string" 
	  HOMEPAGE_URL "https://github.com/VA7ODR/json_document"
	  LANGUAGES CXX
  )

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

add_library(sdstring INTERFACE)

target_sources(sdstring INTERFACE FILE_SET pub_headers TYPE HEADERS BASE_DIRS include/ FILES include/SDString/sdstring.hpp)
target_compile_features(sdstring INTERFACE cxx_std_17)

target_include_directories(sdstring INTERFACE "$<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>"
	                                      "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>")


install(TARGETS sdstring EXPORT SDStringTargets 
	FILE_SET pub_headers DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
	INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(EXPORT SDStringTargets FILE SDStringTargets.cmake NAMESPACE SDString:: DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/SDString)

export(EXPORT SDStringTargets FILE "${CMAKE_CURRENT_BINARY_DIR}/cmake/SDStringTargets.cmake" NAMESPACE SDString::)

configure_package_config_file(${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in
  "${CMAKE_CURRENT_BINARY_DIR}/SDStringConfig.cmake"
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/SDString)

install(FILES
           "${CMAKE_CURRENT_BINARY_DIR}/SDStringConfig.cmake"
	   "${CMAKE_CURRENT_BINARY_DIR}/SDStringConfigVersion.cmake"
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/SDString
)

set_property(TARGET sdstring PROPERTY INTERFACE_SDString_VERSION ${PROJECT_VERSION})
set_property(TARGET sdstring PROPERTY COMPATIBLE_INTERFACE_STRING SDString_VERSION)

write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/SDStringConfigVersion.cmake"
  VERSION "${PROJECT_VERSION}"
  COMPATIBILITY ExactVersion
)
