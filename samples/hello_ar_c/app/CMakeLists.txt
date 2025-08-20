# Copyright 2017 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
##

# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.10)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
# Import the ARCore (Google Play Services for AR) library.
add_library(arcore SHARED IMPORTED)
set_target_properties(arcore PROPERTIES IMPORTED_LOCATION
                      ${ARCORE_LIBPATH}/${ANDROID_ABI}/libarcore_sdk_c.so
                      INTERFACE_INCLUDE_DIRECTORIES ${ARCORE_INCLUDE}
)

# Import the glm header file from the NDK.
add_library( glm INTERFACE )
set_target_properties( glm PROPERTIES INTERFACE_INCLUDE_DIRECTORIES ${GLM_INCLUDE})

# This is the main app library.
add_library(hello_ar_native SHARED
           src/main/cpp/background_renderer.cc
           src/main/cpp/hello_ar_application.cc
           src/main/cpp/jni_interface.cc
           src/main/cpp/obj_renderer.cc
           src/main/cpp/plane_renderer.cc
           src/main/cpp/point_cloud_renderer.cc
           src/main/cpp/texture.cc
           src/main/cpp/util.cc)

target_include_directories(hello_ar_native PRIVATE
           src/main/cpp)


include_directories(src/main/cpp/zxing-cpp/core/src)

add_library(zxing STATIC IMPORTED)
set_target_properties(zxing PROPERTIES IMPORTED_LOCATION
        C:/Users/yanhuizhe/StudioProjects/yrarcore/samples/hello_ar_c/app/src/main/cpp/zxing-cpp/${ANDROID_ABI}/core/libZXing.a
)

set(OpenCV_DIR C:/Users/yanhuizhe/StudioProjects/yrarcore/samples/hello_ar_c/app/src/main/cpp/opencv-mobile-2.4.13.7-android/sdk/native/jni)
find_package(OpenCV REQUIRED core imgproc)


target_link_libraries(hello_ar_native
                      android
                      log
                      GLESv2
                      glm
        ${OpenCV_LIBS}
        zxing
                      arcore)
