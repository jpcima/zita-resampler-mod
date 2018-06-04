cmake_minimum_required(VERSION 3.5)

project(zita-resampler VERSION "1.6.0" LANGUAGES CXX)

add_library(zita-resampler SHARED
  "libs/cresampler.cc"
  "libs/resampler.cc"
  "libs/resampler-table.cc"
  "libs/vresampler.cc")
target_include_directories(zita-resampler
  PUBLIC "${PROJECT_SOURCE_DIR}/libs")
set_target_properties(zita-resampler PROPERTIES
  SOVERSION "${PROJECT_VERSION}")
if(NOT CMAKE_SYSTEM_NAME STREQUAL "Windows")
  find_package(Threads REQUIRED)
  target_link_libraries(zita-resampler PUBLIC ${CMAKE_THREAD_LIBS_INIT})
endif()

install(TARGETS zita-resampler
  LIBRARY DESTINATION "lib"
  ARCHIVE DESTINATION "lib"
  RUNTIME DESTINATION "bin")
install(DIRECTORY "libs/zita-resampler"
  DESTINATION "include")
