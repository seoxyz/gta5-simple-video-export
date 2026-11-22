find_package(spdlog CONFIG REQUIRED)
find_package(PolyHook_2 CONFIG REQUIRED)

if(TARGET PolyHook_2::PolyHook_2)
    message(STATUS "PolyHook target: PolyHook_2::PolyHook_2")
elseif(TARGET PolyHook_2)
    message(STATUS "PolyHook target: PolyHook_2")
elseif(TARGET PolyHook_2::PolyHook_2_static)
    message(STATUS "PolyHook target: PolyHook_2::PolyHook_2_static")
else()
    message(STATUS "PolyHook targets not found. Available targets:")
    get_property(allTargets DIRECTORY PROPERTY BUILDSYSTEM_TARGETS)
    message(STATUS "${allTargets}")
endif()

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
    PolyHook_2
    spdlog::spdlog
)
