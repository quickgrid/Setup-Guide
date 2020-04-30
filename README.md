# Build-Guide
A build and setup guide for various libraries such as Dlib, OpenCV, NCNN etc for Windows C++, Python and Android C++, Java etc.

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


## Projects Tested

These are some of the projects I have tested. Most or all of these can be implemented in both pc and android. 

| Name | Link |
| --- | --- |
| Very fast and accurate rotation-invariant face detector | https://github.com/MagicJackStone/FaceKit/tree/master/PCN |
| Ultra light face detector OpenCV DNN | https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master/opencv_dnn |
| Ultra light face detector NCNN | https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB/tree/master/ncnn/src |
| Object Tracking | https://www.learnopencv.com/object-tracking-using-opencv-cpp-python/ |
| Facial Landmarks with Dlib | http://dlib.net/face_landmark_detection_ex.cpp.html |
| Facial Landmarks with OpenCV Contrib | https://docs.opencv.org/master/de/d27/tutorial_table_of_content_face.html <br> https://github.com/kurnianggoro/GSOC2017 |
| YoloFace face detection | https://github.com/sthanhng/yoloface |
| OpenCV DNN Object Detection and Localizaiton | Pretrained darknet yolo (cfg, weights), Caffe (prototxt, caffemodel) etc. |


## Android

Note in order to make latest version of all three working in same project the easiest way for beginner is to setup opencv contrib/opencv, next setup dlib after changing target project name, cmake, ndk paths, finally setup ncnn.

`NOTE:` Must create C++ android project to make these all work together.

### OpenCV Contrib

Build instructions for opencv contrib,

https://medium.com/beesightsoft/build-opencv-opencv-contrib-for-android-on-windows-9894b4fe6386

But easiest way at the moment is download prebuilt version of latest opencv contrib from,

https://pullrequest.opencv.org/buildbot/export/opencv_releases/master-contrib_pack-contrib-android/

Use the following link and template to setup native opencv project for android. Instead of using opencv android from their site just replace it with opencv contrib from the link above.

https://github.com/VlSomers/native-opencv-android-template

#### Sample OpenCV DNN Setup

https://github.com/ivangrov/Android-Deep-Learning-with-OpenCV


### Dlib

Download dlib from github repo and extract to `C:` drive such that extracted path is `C:\dlib\dlib-master\`.

Use this [dlib powershell script](dlib-android-setup.ps1) to copy dlib directly into the project. Must change the absolute paths before running the script. Change `$CMAKE_BIN_PATH`, `$NDK`, `$PROJECT_PATH`, `$DLIB_PATH`.

#### Execution Method

Paste the script in `C:\dlib\` and run powershell as administrator. Paste `set-location C:\dlib\dlib-master\` and press enter to go into that folder. Run `Set-ExecutionPolicy RemoteSigned` and press `A`. 

Next, paste `& "C:\dlib\dlib-android-setup.ps1` and enter to start compiling and copying files to your project. Must change paths accordingly in script before execution.

After all dlib files are copied to android project, paste `Set-ExecutionPolicy Restricted` to powershell and press `A`.


Further detail on script and execution procedure, 

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

For `Visual Studio 2019` Use opencv_worldXXX.lib, Ex: `opencv_world430.lib` for release configuration and `opencv_world430d.lib` for debug configuration. If opencv is extracted to `C:` then it can be fould in `C:\opencv\build\x64\vc15\lib`.

### Dlib

### NCNN

https://github.com/Tencent/ncnn/wiki/how-to-build
