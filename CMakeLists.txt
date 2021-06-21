cmake_minimum_required(VERSION 3.5)

project(zita-resampler VERSION "1.6.0" LANGUAGES CXX)

option(ZITA_RESAMPLER_ENABLE_SHARED "Build as a shared library" ON)
option(ZITA_RESAMPLER_ENABLE_STATIC "Build as a static library" ON)
option(ZITA_RESAMPLER_ENABLE_SSE2 "Build vectorized code with SSE2" OFF)

set(ZITA_RESAMPLER_SOURCES
  "source/cresampler.cc"
  "source/resampler.cc"
  "source/resampler-table.cc"
  "source/vresampler.cc")

find_package(Threads REQUIRED)

set(ZITA_RESAMPLER_TARGETS)

if(ZITA_RESAMPLER_ENABLE_SHARED)
  add_library(zita-resampler SHARED ${ZITA_RESAMPLER_SOURCES})
  target_include_directories(zita-resampler
    PUBLIC "${PROJECT_SOURCE_DIR}/source")
  target_link_libraries(zita-resampler PUBLIC ${CMAKE_THREAD_LIBS_INIT})
  set_target_properties(zita-resampler PROPERTIES
    SOVERSION "${PROJECT_VERSION}")
  list(APPEND ZITA_RESAMPLER_TARGETS zita-resampler)
  if(ZITA_RESAMPLER_ENABLE_SSE2)
    target_compile_definitions(zita-resampler PRIVATE "ENABLE_SSE2")
  endif()
endif()

if(ZITA_RESAMPLER_ENABLE_STATIC)
  add_library(zita-resampler-static STATIC ${ZITA_RESAMPLER_SOURCES})
  target_include_directories(zita-resampler-static
    PUBLIC "${PROJECT_SOURCE_DIR}/source")
  target_link_libraries(zita-resampler-static PUBLIC ${CMAKE_THREAD_LIBS_INIT})
  set_target_properties(zita-resampler-static PROPERTIES
    OUTPUT_NAME "zita-resampler")
  list(APPEND ZITA_RESAMPLER_TARGETS zita-resampler-static)
  if(ZITA_RESAMPLER_ENABLE_SSE2)
    target_compile_definitions(zita-resampler-static PRIVATE "ENABLE_SSE2")
  endif()
endif()

install(TARGETS ${ZITA_RESAMPLER_TARGETS}
  LIBRARY DESTINATION "lib"
  ARCHIVE DESTINATION "lib"
  RUNTIME DESTINATION "bin")
install(DIRECTORY "source/zita-resampler"
  DESTINATION "include")
