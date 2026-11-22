find_package(spdlog CONFIG REQUIRED)
find_package(FFMPEG REQUIRED)
find_package(PolyHook_2 CONFIG REQUIRED)

add_library(common STATIC
  audiostream.cpp
  avcreate.cpp
  format.cpp
  logger.cpp
  settings.cpp
  stream.cpp
  videostream.cpp)

target_include_directories(common PUBLIC ${FFMPEG_INCLUDE_DIRS})
target_link_directories(common PUBLIC ${FFMPEG_LIBRARY_DIRS})
target_link_libraries(common PUBLIC ${FFMPEG_LIBRARIES})

if(TARGET PolyHook_2::PolyHook_2)
  get_target_property(_ph2_type PolyHook_2::PolyHook_2 TYPE)
  if(_ph2_type STREQUAL "EXECUTABLE")
    # PolyHook_2::PolyHook_2 terdeteksi sebagai executable, bukan library
    find_library(POLYHOOK2_LIB NAMES PolyHook_2 PolyHook2 polyhook2 PATHS "${CMAKE_PREFIX_PATH}/lib" "${CMAKE_SOURCE_DIR}/deps/lib" NO_DEFAULT_PATH)
    find_library(ZYDIS_LIB NAMES Zydis zydis PATHS "${CMAKE_PREFIX_PATH}/lib" NO_DEFAULT_PATH)
    find_library(ZYCORE_LIB NAMES Zycore zycore PATHS "${CMAKE_PREFIX_PATH}/lib" NO_DEFAULT_PATH)
    target_include_directories(common PUBLIC "${CMAKE_PREFIX_PATH}/include")
    if(POLYHOOK2_LIB)
      target_link_libraries(common PUBLIC "${POLYHOOK2_LIB}")
    else()
      message(FATAL_ERROR "Library PolyHook2 (.lib) tidak ditemukan di deps/lib")
    endif()
    if(ZYDIS_LIB)
      target_link_libraries(common PUBLIC "${ZYDIS_LIB}")
    endif()
    if(ZYCORE_LIB)
      target_link_libraries(common PUBLIC "${ZYCORE_LIB}")
    endif()
  else()
    target_link_libraries(common PUBLIC PolyHook_2::PolyHook_2)
  endif()
endif()


target_link_libraries(common PUBLIC spdlog::spdlog)
