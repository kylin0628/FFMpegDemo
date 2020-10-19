# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.10.2)

project("ffmpegdemo")

add_library( # Sets the name of the library.
        native-lib
        SHARED
        native-lib.cpp)

find_library( # Sets the name of the path variable.
        log-lib
        log)

set(FF ${CMAKE_CURRENT_SOURCE_DIR}/../../../libs)
add_library(avcodec SHARED IMPORTED)
add_library(avformat SHARED IMPORTED)
add_library(avutil SHARED IMPORTED)
add_library(swscale SHARED IMPORTED)
add_library(swresample SHARED IMPORTED)
set_target_properties(avcodec PROPERTIES IMPORTED_LOCATION ${FF}/${ANDROID_ABI}/libavcodec.so)
set_target_properties(avformat PROPERTIES IMPORTED_LOCATION ${FF}/${ANDROID_ABI}/libavformat.so)
set_target_properties(avutil PROPERTIES IMPORTED_LOCATION ${FF}/${ANDROID_ABI}/libavutil.so)
set_target_properties(swscale PROPERTIES IMPORTED_LOCATION ${FF}/${ANDROID_ABI}/libswscale.so)
set_target_properties(swresample PROPERTIES IMPORTED_LOCATION ${FF}/${ANDROID_ABI}/libswresample.so)

target_include_directories(native-lib PRIVATE
        src/main/cpp/include)


target_link_libraries( # Specifies the target library.
        native-lib
        avcodec avformat avutil swscale swresample
        ${log-lib})