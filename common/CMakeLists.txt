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

# Jika PolyHook_2::PolyHook_2 ada, pakai langsung; jika tidak, cek alternatif nama target
if(TARGET PolyHook_2::PolyHook_2)
  target_link_libraries(common PUBLIC PolyHook_2::PolyHook_2)
elseif(TARGET PolyHook_2::PolyHook2)
  target_link_libraries(common PUBLIC PolyHook_2::PolyHook2)
elseif(TARGET PolyHook2)
  target_link_libraries(common PUBLIC PolyHook2)
else()
  message(FATAL_ERROR "Target PolyHook_2 tidak ditemukan setelah find_package(PolyHook_2)")
endif()

target_link_libraries(common PUBLIC spdlog::spdlog)
