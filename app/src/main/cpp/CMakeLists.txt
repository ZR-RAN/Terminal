# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.10.2)

# Declares and names the project.

project("terminal")

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

add_library( # Sets the name of the library.
    libjni_terminal

    # Sets the library as a shared library.
    SHARED

    # Provides a relative path to your source file(s).
    jni_init.cpp com_android_terminal_Terminal.cpp )

add_library(
    libvterm
    STATIC
    libvterm/src/encoding.c
    libvterm/src/input.c
    libvterm/src/keyboard.c
    libvterm/src/mouse.c
    libvterm/src/parser.c
    libvterm/src/pen.c
    libvterm/src/screen.c
    libvterm/src/state.c
    libvterm/src/unicode.c
    libvterm/src/vterm.c )

include_directories(
    SYSTEM
    PUBLIC binder/include
    PUBLIC liblog/include_vndk
    PUBLIC libutils/include
    PUBLIC core/jni/include
    PUBLIC libnativehelper/include
    PUBLIC libnativehelper/header_only_include
    PUBLIC libsystem/include
    PUBLIC libvterm/include )

#target_link_libraries(
#    libjni_terminal
#    -Llib/aarch64
#    -landroidfw
#    -landroid_runtime
#    -llog
#    -lnativehelper
#    -lutils )

add_library(
    libandroidfw
    SHARED
    IMPORTED )

add_library(
    libandroid_runtime
    SHARED
    IMPORTED )

add_library(
    liblog
    SHARED
    IMPORTED )

add_library(
    libnativehelper
    SHARED
    IMPORTED )

add_library(
    libutils
    SHARED
    IMPORTED )

set_target_properties(
    libandroidfw PROPERTIES IMPORTED_LOCATION
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/aarch64/libandroidfw.so )

set_target_properties(
    libandroid_runtime PROPERTIES IMPORTED_LOCATION
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/aarch64/libandroid_runtime.so )

set_target_properties(
    liblog PROPERTIES IMPORTED_LOCATION
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/aarch64/liblog.so )

set_target_properties(
    libnativehelper PROPERTIES IMPORTED_LOCATION
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/aarch64/libnativehelper.so )

set_target_properties(
    libutils PROPERTIES IMPORTED_LOCATION
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/aarch64/libutils.so )

target_link_libraries(
    libjni_terminal
    libandroidfw
    libandroid_runtime
    liblog
    libnativehelper
    libutils )

target_link_libraries(
    libjni_terminal
    libvterm )

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

#find_library( # Sets the name of the path variable.
#              log-lib
#
#              # Specifies the name of the NDK library that
#              # you want CMake to locate.
#              log )

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

#target_link_libraries( # Specifies the target library.
#                       native-lib
#
#                       # Links the target library to the log library
#                       # included in the NDK.
#                       ${log-lib} )