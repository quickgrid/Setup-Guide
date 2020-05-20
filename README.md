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

<br>

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

<br>

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

Next, paste `& "C:\dlib\dlib-android-setup.ps1"` and enter to start compiling and copying files to your project. Must change paths accordingly in script before execution.

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

<br>

## Windows

### OpenCV Contrib

For `Visual Studio 2019` Use opencv_worldXXX.lib, Ex: `opencv_world430.lib` for release configuration and `opencv_world430d.lib` for debug configuration. If opencv is extracted to `C:` then it can be fould in `C:\opencv\build\x64\vc15\lib`.

### Dlib

Compile instructions,

http://dlib.net/compile.html

For use cmake to configure and Visual Studio compile ALL_BUILD dlib in release x64 to generate dlib_(...).lib.

In desired project set vc++ include directories, include path to dlib master downloaded from master to extracted folder. Set library directoies to generated `dlib.lib`. Copy `dlib/all/source.cpp` to visual studio source files folder where other c++ codes reside.

To enable jpg support set C/C++ > General > Additional Include Directories, to extracted dlib folder something like, `C:\dlib-master\dlib\external\libjpeg`.  Set C/C++ > Preprocessor > Preprocessor definitions, to `CRT_SECURE_NO_WARNINGS` and `DLIB_JPEG_SUPPORT` for jpg support.

Example for testing, http://dlib.net/dnn_face_recognition_ex.cpp.html

### NCNN

https://github.com/Tencent/ncnn/wiki/how-to-build

<br>

## Windows 10 LLVM Clang, Ninja, CMake Generate C++ Executable

### Tool Links

Ninja Windows Release, https://github.com/ninja-build/ninja/releases

LLVM Download, https://releases.llvm.org/download.html

### Process

Make a build folder to store generated files. In cmake set this path. Also set path for source `C++` folder. Click configure and select generator. Choose Ninja and tick specify native compilers. Set LLVM bin clang.exe and clang++.exe for C and C++.

There will be error that `CMAKE_MAKE_PROGRAM-NOTFOUND`. Browse to directory where ninja release was extracted and select `ninja.exe`. It should also be possible (not tested) to use ninja.exe that comes with android studio setup in android cmake folder.

Now press configure and again configure if all looks correct. Next, click generate. Open cmd navigate to project source folder and run `ninja.exe`. If not is system path then specify location as, "C:\ninja-win\ninja.exe" on command prompt.

### Minimal Example

https://stackoverflow.com/questions/52760801/cmake-building-for-windows-clang-cl-using-ninja-generator

#### CMakeLists.txt

```
cmake_minimum_required(VERSION 3.12)
project(hello_world)
add_executable(${PROJECT_NAME} main.cpp)
```

#### main.cpp

```cpp
#include <iostream>

using namespace std;

int main(){

    cout << "HELLO" << "\n";

    cin.get();

    return 0;
}
```

<br>

## Windows 10 CMU `openpose` Setup Visual Studio 2019, CMake, Nvidia GPU

This setup is quite `complex`.

### Prerequisite

First read these carefully,

https://github.com/CMU-Perceptual-Computing-Lab/openpose#installation-reinstallation-and-uninstallation

https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/doc/prerequisites.md

https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/doc/installation.md

### Build Process

Ensure CUDA, cuDNN is setup in system properly. In my case CUDA 10.2 and cuDNN 7.6.5 for 10.2 x64 works.

Download the source from github as ZIP, https://github.com/CMU-Perceptual-Computing-Lab/openpose. Maybe git recursive clone is better. Extract it a folder. Ex: `C:\openpose-master`.

If 3rd party dependencies are not in extracted folder. Then download those project zip separately and extract to approprite folder. Ex: `pybind11`, `caffe`.

In `openpose-master\3rdparty\windows` use the `bat` files to download dependencies. If download is slow, open the batch files and manually download each zip files with given url and extract to folders `caffe`, `caffe3rdparty`, `opencv`, `freeglut`. For manual downloads the downloaded files may have extra characters added to them, so change their name as above.

Create a new folder inside `openpose-master` named `build`. Point to source folder(`C:\openpose-master`) and build(`C:\openpose-master\build`) folder in CMake GUI. Press `configure` and choose visual studio 2019 as generator and platform as x64 then press finish. Wait for cmake to configure. CMake should automatically find everything. Next, press configure again and press generate. It should not give any errors. 

If there is any error it has to with path to `caffe`, `caffe3rdparty`, `opencv`, `freeglut`, or they were not downloaded properly. If they are in custom path set each or their value until `generate` button no longer gives any error.

Open build folder now and open the `*.sln` file with visual studio. Change configuration to `Release` and in solution explorer run `ALL_BUILD` target or run `Local Windows Debugger` directly.

In order to run openpose samples create a new c++ empty project in VS2019. Set configuration to `Release`. Add desired sample cpp code to run from, https://github.com/CMU-Perceptual-Computing-Lab/openpose/tree/master/examples/tutorial_api_cpp to `Source Files` folder in project.

Next step in to add `*.dll`, `*.lib` files and necessary include folders via project `Properties`. In VC++ include directories, set path to all include folder in openpose-master source folders. Ex:

`C:\openpose-master\include`
`C:\openpose-master\3rdparty\windows\opencv\include`

Do this for all include folder in 3rdparty folder. Next, set Library directories,

`C:\openpose-master\build\src\openpose\Release`
`C:\openpose-master\3rdparty\windows\opencv\x64\vc15\lib`

Do same for all lib folders in 3rdparty directory. Next, write name of each `*.lib` files in `Linker > Input > Additional Dependencies` such as, 

```
freeglut.lib
glog.lib
gflags.lib
caffe.lib
caffeproto.lib
opencv_world420.lib
openpose.lib
```

(Optional) Use the `d` suffix versions for debug config if debugging config is set, otherwise use above for release.

Copy models folder from openpose source to VS2019 solution project folder. It will have caffemodels and prototxt but caffemodels are placeholder. If batch was used to run then it should have everything ready. Otherwise paste the downloaded caffe models here for different tasks which were downloaded with `*.bat` links manually. 

In the demo cpp code set path to image file correctly before running. Run the code and it will give error that `*.dll` not found. In Visual Studio solution x64 folder in explorer where `*.exe` is found, paste the dll files from here,

`C:\openpose-master\build\bin` 

Also paste, `openpose.dll` from build folder. Search for it not found. Now each sample codes should run directly with image/video path change.


