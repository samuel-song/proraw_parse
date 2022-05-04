cmake_minimum_required(VERSION 3.15 FATAL_ERROR)
project(dng_sdk LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)

if(NOT CMAKE_BUILD_TYPE)
message("set release default")
set(CMAKE_BUILD_TYPE Release)
endif()


file(GLOB_RECURSE SRC_FILES "${CMAKE_SOURCE_DIR}/src/*.cpp")

add_executable(validate ${SRC_FILES})


set(XMP_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/xmp_lib/include")
set(XMP_LIBRARIES_RELEASE "${CMAKE_SOURCE_DIR}/xmp_lib/lib/XMPCoreStaticRelease.lib" "${CMAKE_SOURCE_DIR}/xmp_lib/lib/XMPFilesStaticRelease.lib")
set(XMP_LIBRARIES_DEBUG "${CMAKE_SOURCE_DIR}/xmp_lib/lib/XMPCoreStaticDebug.lib" "${CMAKE_SOURCE_DIR}/xmp_lib/lib/XMPFilesStaticDebug.lib")
set(JPEG_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/jpeg9c_lib/include")
set(JPEG_LIBRARIES_RELEASE "${CMAKE_SOURCE_DIR}/jpeg9c_lib/libjpeg.lib")
set(JPEG_LIBRARIES_DEBUG "${CMAKE_SOURCE_DIR}/jpeg9c_lib/libjpeg_d.lib")
set(DNG_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/dng_sdk/include")
set(DNG_LIBRARIES_RELEASE "${CMAKE_SOURCE_DIR}/dng_sdk/dng_sdk_x64_Release.lib")
set(DNG_LIBRARIES_DEBUG "${CMAKE_SOURCE_DIR}/dng_sdk/dng_sdk_x64_Debug.lib")



include_directories(${XMP_INCLUDE_DIR} ${JPEG_INCLUDE_DIR} ${DNG_INCLUDE_DIR})


target_compile_definitions(validate PUBLIC "qDNGValidate=1;"
    "qDNGValidateTarget=1;"
    "$<$<CONFIG:Debug>:"
    "qDNGDebug=1;"
    ">"
    "$<$<CONFIG:Release>:"
    "qDNGDebug=0;"
    ">"
    "$<$<CONFIG:Debug>:"
    "_DEBUG;"
    ">"
    "$<$<CONFIG:Release>:"
    "NDEBUG;"
    ">"
    "_CONSOLE;"
    "_UNICODE;"
    "WIN64;"
    "WIN32;"
    "qiPhone=0;"
    "qAndroid=0;"
    "qWinOS=1;"
    "qMacOS=0;"
    "qVisualC=0;"
    "qWeb=0;"
    "qWinRT=0;"
    "qLinux=0;"
    "_WINDOWS=1;"
    "BIB_MULTI_THREAD=1;"
    "qDNGUseStdInt=1;"
    "qDNGPrintMessages=1;"
    "qDNGPreferGPSMetadataFromXMP=1")

if(CMAKE_BUILD_TYPE STREQUAL "Release")
message("release link")
target_link_libraries(validate ${XMP_LIBRARIES_RELEASE} ${JPEG_LIBRARIES_RELEASE} ${DNG_LIBRARIES_RELEASE})
elseif(CMAKE_BUILD_TYPE STREQUAL "Debug")
message("debug link")
target_link_libraries(validate ${XMP_LIBRARIES_DEBUG} ${JPEG_LIBRARIES_DEBUG} ${DNG_LIBRARIES_DEBUG})
endif()