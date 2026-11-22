find_package(spdlog CONFIG REQUIRED)
find_package(PolyHook_2 CONFIG REQUIRED)

add_library(common STATIC
    audiostream.cpp
    avcreate.cpp
    format.cpp
    logger.cpp
    settings.cpp
    stream.cpp
    videostream.cpp
)

target_include_directories(common PUBLIC
    "${CMAKE_SOURCE_DIR}/deps/include"
)

target_link_directories(common PUBLIC
    "${CMAKE_SOURCE_DIR}/deps/lib"
)

target_link_libraries(common PUBLIC
    avcodec
    avformat
    avutil
    swresample
    swscale
    PolyHook_2::PolyHook_2
    spdlog::spdlog
)
