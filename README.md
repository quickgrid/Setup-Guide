# Build-Guide
A build guide for various libraries such as Dlib, OpenCV, NCNN etc for Windows, Android etc.

## Tools Required

Download the installer or zip version of cmake. Dlib 19.19 from official website had bug with opencv `cv_image` which is fixed on github source.

- CMake, https://cmake.org/download/
- Visual Studio 2019 Community Edition,
- Vulkan SDK, https://vulkan.lunarg.com/sdk/home
- Cuda and cuDNN with `CUDA_PATH` environment variable set
- Dlib source code from github, https://github.com/davisking/dlib
- Anaconda, https://www.anaconda.com/products/individual
- NCNN source, https://github.com/Tencent/ncnn
- OpenCV source, https://github.com/opencv/opencv
- OpenCV contrib source, https://github.com/opencv/opencv_contrib


## Implementable Projects

These can be implemented in both pc and android.

| Name | Link |
| --- | --- |
| Very fast and accurate rotation-invariant face detector | https://github.com/MagicJackStone/FaceKit/tree/master/PCN |
| Ultra light face detector OpenCV DNN | https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master/opencv_dnn |
| Ultra light face detector NCNN | https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master/ncnn/src |
| Object Tracking | https://www.learnopencv.com/object-tracking-using-opencv-cpp-python/ |
| Facial Landmarks with Dlib | http://dlib.net/face_landmark_detection_ex.cpp.html |
| Facial Landmarks with OpenCV Contrib | https://docs.opencv.org/master/de/d27/tutorial_table_of_content_face.html <br> https://github.com/kurnianggoro/GSOC2017 |

## Android

Note in order to make latest version of all three working in same project the easiest way for beginner is to setup opencv contrib/opencv, next setup dlib after changing target project name, cmake, ndk paths, finally setup ncnn.

`NOTE:` Must create C++ android project to make these all work together.

### OpenCV Contrib

Build instructions for opencv contrib,

https://medium.com/beesightsoft/build-opencv-opencv-contrib-for-android-on-windows-9894b4fe6386

But easiest way at the moment is download prebuilt version of latest opencv contrib from,

https://pullrequest.opencv.org/buildbot/export/opencv_releases/master-contrib_pack-contrib-android/

Use the following link and template to setup native opencv project for android,

https://github.com/VlSomers/native-opencv-android-template

### Dlib

Get dlib master source code from github and compile with this script after changing paths, 

https://stackoverflow.com/questions/60548479/setting-up-dlib-for-android-studio/60550358#60550358

Original script source,

https://github.com/Luca96/dlib-for-android


### NCNN

https://github.com/Tencent/ncnn/wiki/how-to-build

Easiest option for android is download prebuilt releases,

https://github.com/Tencent/ncnn/releases

Download vulkan library for GPU support.

## Windows

### OpenCV Contrib

### Dlib

### NCNN

https://github.com/Tencent/ncnn/wiki/how-to-build
