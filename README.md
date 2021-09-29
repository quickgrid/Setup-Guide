# Setup-Guide

A build and setup guide for various libraries, tools, cloud usage etc.


## Tools Required

Download the installer or zip version of cmake. Dlib 19.19 from official website had bug with opencv `cv_image` which is fixed on github source.

- CMake, https://cmake.org/download/
- Visual Studio 2019 Community Edition
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

### OpenCV Contrib Windows Build From Source For Python Anaconda

Download `opencv`, `opencv_contrib`, `cmake`. Extract and paste both into a folder for path, ex: `opencv-compilation`. Use `cmake-gui` browse source to point to opencv folder and click configure. Find `OPENCV_EXTRA_MODULES_PATH` and point to opencv_contrib modules folder. Point `PYTHON3_` variables to correct conda environment path. Untick `BUILD_SHARED_LIBS`, tick `BUILD_opencv_world`, click configure and generate. It may be necessary to have numpy installed beforehand. Unchecking unncessary modules will speed up process if python is the only target. 

This will generate `build` folder inside `opencv-compilation`. Open `OpenCV.sln`, choose `release` configuration `x64`. In solution explorer open `CMakeTargets`, right click on `ALL_BUILD` and click build. After compilation done copy `cv2.****.pyd` file from `opencv-compilation\build\lib\python3\Release` to Anaconda or Miniconda custom enviroment. For examples, I used miniconda3 and the copy location for `pyd` file was `C:\Users\...\miniconda3\envs\mycustomenv\Lib\site-packages`.

Check installation by activating correct environment and running,
```
import cv2
print(cv2.__version__)
```

### OpenCV Contrib Application

For `Visual Studio 2019` Use opencv_worldXXX.lib, Ex: `opencv_world430.lib` for release configuration and `opencv_world430d.lib` for debug configuration. If opencv is extracted to `C:` then it can be fould in `C:\opencv\build\x64\vc15\lib`.

Depending on CPU cores CUDA compilation can be very slow. OpenCV CUDA build,

https://haroonshakeel.medium.com/build-opencv-4-4-0-with-cuda-gpu-support-on-windows-10-without-tears-aa85d470bcd0.

https://www.pyimagesearch.com/2020/02/03/how-to-use-opencvs-dnn-module-with-nvidia-gpus-cuda-and-cudnn/

OpenCV python CUDA static build,

https://github.com/opencv/opencv/issues/20206.

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

## VSCodium Windows C++ Build and Debugging with `g++` and `gdb`

Instructions for setting up c++ build and debugging on windows. For MinGW `gcc, g++, gdb` etc. I used Codeblocks MinGW version (may cause problem with `std::thread`) where in `bin` folder they are available.
Sample path is, `{base_path}\codeblocks\MinGW\bin`. Need to have Microsoft `C/C++` extensions installed in vscodium.

https://code.visualstudio.com/docs/cpp/config-mingw

To create workspace add vscodium to path and use `codium .`. Create desired cpp file in the workspace. Next, choose `Terminal > Configure Tasks > Shell:g++.exe build active file`. This will create a `build.json` file and can be run from `Terminal > Run Build Task..`. Next, from `Run > Add Configurations` choose `C++ (GDB/LLDB) > g++.exe build and debug active file`.

Now, set breakpoints and use `Run > Start Debugging`. If there is any error with `preLaunchTask` in `launch.json`, then matching with `task.json` label should fix it.

This method with `cl.exe` works for build, but debugging does not work.

https://code.visualstudio.com/docs/cpp/config-msvc

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

If there is any error, it has to with path to `caffe`, `caffe3rdparty`, `opencv`, `freeglut`, or they were not downloaded properly. If they are in custom path set each or their value until `generate` button no longer gives any error.

Open build folder now and open the `*.sln` file with visual studio. Change configuration to `Release` and in solution explorer run `ALL_BUILD` target or run `Local Windows Debugger` directly.

In order to run openpose samples create a new c++ empty project in VS2019. Set configuration to `Release`. Add desired sample cpp code to run from, https://github.com/CMU-Perceptual-Computing-Lab/openpose/tree/master/examples/tutorial_api_cpp to `Source Files` folder in project.

Next step in to add `*.dll`, `*.lib` files and necessary include folders via project `Properties`. In VC++ include directories, set path to all include folder in openpose-master source folders. Ex:

```
C:\openpose-master\include
C:\openpose-master\3rdparty\windows\opencv\include
```

Do this for all include folder in 3rdparty folder. Next, set Library directories,

```
C:\openpose-master\build\src\openpose\Release
C:\openpose-master\3rdparty\windows\opencv\x64\vc15\lib
```

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

#### Face Keypoint Sample Image

https://github.com/quickgrid/Build-Guide/blob/master/openpose_face_keypoints_sample.jpg

<br>

## AWS EC2 Ubuntu GUI, CLI from Windows for Tensorflow Lite Build

### Required

TightVNC Viewer, https://www.tightvnc.com/download.php

RealVNC Viewer (Alternative to TightVNC), https://www.realvnc.com/en/

Putty, https://www.putty.org/

WinSCP , https://winscp.net/eng/index.php

### Instructions

Use `Shift + Insert` key to paste copied text from windows clipboard to Putty ubuntu terminal `vim`. Use `Shift + :` for inserting commands, `:q!` for exiting without saving. Use AWS `Public IPv4 DNS` in Putty hostname. Use `puttygen` to convert `pem` file to `ppk` to be used by putty, click `save private key`. In putty `SSH > Auth` section browse to `ppk` file and click open to connect. 

Use `ubuntu` as username for default ubuntu image. Instructions for installing and connecting to GUI is available here.  

https://ubuntu.com/tutorials/ubuntu-desktop-aws

It will also work for windows host. Paste all the required commands in putty terminal. If all went correctly use the `IPv4 DNS` address appended with `:1` to connect to ubuntu GUI from tightVNC. Make sure to set up inbound rule for VNC access.

Use `df -BM` for checking free space.

### Docker Installation

Run these commands one by one on `putty terminal`. Tested on ubuntu `18.04` ec2 instance. Full instructions here, https://phoenixnap.com/kb/install-docker-on-ubuntu-20-04.

```
sudo apt update
```

```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu  $(lsb_release -cs)  stable"
```

```
sudo apt update
```

```
sudo apt-get install docker-ce
```

```
docker --version
```

```
sudo systemctl start docker
```

```
sudo systemctl enable docker
```

```
sudo systemctl status docker
```

Now, docker is ready for android tensorflow lite build.


### Setup for Tensorflow Lite Android Build on EC2

Build instructions here, https://www.tensorflow.org/lite/guide/build_android.

To fix issue with `Got permission denied while trying to connect to the docker daemon socket at unix:///var/run/docker.sock: .....` use the command `sudo chmod 666 /var/run/docker.sock`.

Within docker type `ls` to get folder list and navigate to `tensorflow_src` with `cd tensorflow_src`. Here, type `./configure` to configure tensorflow build.

Enter an exited docker container with, `docker start 7263550e2520 && docker attach 7263550e2520`. To start the last one use, ``docker start `docker ps -q -l` && docker attach `docker ps -q -l` ``.

To get list of stopped containers with information use, `docker ps -a`. 

### File Transfer from EC2 to Local System

Use `WinSCP` to download files from aws ec2 to local system. Download the android tensorflow `aar` files with this to put into android project.

Use `ppk` generated with `puttygen` in WinSCP advanced authentication. Log in to server with user name.

WinSCP AWS EC2 instructions, https://winscp.net/eng/docs/guide_amazon_ec2.

### Keep Terminal Session alive after closing Putty

Use `screen` to detach and run commands. Install with,

`apt-get install screen`

Enter `screen` on terminal and `ctrl + a + d` to detach. Use `screen -ls` for a screen list and `screen -r` to resume single screen. For multiple screens use `screen -r <number>` to attach.

Source, https://www.interserver.net/tips/kb/using-screen-to-attach-and-detach-console-sessions/


### Reduced Size AAR Building Resources

https://github.com/tensorflow/tensorflow/blob/master/.bazelrc

https://docs.bazel.build/versions/master/user-manual.html

https://github.com/tensorflow/tensorflow/issues/35170

https://www.tensorflow.org/lite/guide/ops_select

https://www.tensorflow.org/lite/guide/reduce_binary_size

https://www.tensorflow.org/lite/guide/build_android

https://github.com/tensorflow/tensorflow/blob/v2.3.0/tensorflow/lite/g3doc/guide/ops_select.md




<br>

## YOLO Darknet remove accuracy part from being drawn on predicted output image

Source code link for the part, https://github.com/AlexeyAB/darknet/blob/master/src/image.c

Here, as of current date the code is as below on the `draw_detections_v3` function. Commenting out these two lines will prevent accuracy string from being drawn on `predictions.jpg` image. Similarly class output or both can also be removed but why do so?

```
sprintf(prob_str, ": %.2f", selected_detections[i].det.prob[selected_detections[i].best_class]);
strcat(labelstr, prob_str);
```

This should be done before doing `make`. After `make` command there will be a file named `darknet` within darknet folder.

<br>

## Setup Darknet training and inference on Sagemaker notebook instance

For GPU support use a gpu instance for example, ml.p3.2xlarge, is 16gb Nvidia V100 gpu. If vpc not required then select `no vpc`. Start the instance and run `Open JupyterLab`. The main reason for writing this is there is an error with `make` when makefile contains `OPENCV=1`. A working but poor solution is set `OPENCV=0` before make, but this will slow down training and inference as pointed out in output log. For kernel `conda_python3` will work. 

TODO: Use `yum` to install opencv on system to see if opencv problem is solved.  


<br>

## Python draw complex font text like bengali in image correctly

Easiest option in windows is to download libraqm from here, https://www.lfd.uci.edu/~gohlke/pythonlibs/#pillow. I have tested on `miniconda python 3.7` custom environment. To use this put `*.dll` from above link in same folder as `python.exe` in miniconda/anaconda `envs/MY_ENV_NAME` folder. If the code below gives `True` then it will render `Bangla` fonts properly.

```
from PIL import features
print(features.check("raqm"))
```

Windows install instructions, 

https://stackoverflow.com/questions/62939101/how-to-install-pre-built-pillow-wheel-with-libraqm-dlls-on-windows.

Based on answer from, 

https://stackoverflow.com/questions/66184573/how-do-i-install-libraqm-library-in-google-colab.

This script contains example in google colab,

https://github.com/quickgrid/CodeLab/blob/master/colab/Pillow_Render_Bangla_Font_Text_to_Image_libraqm.ipynb

<br>

## Blender black and white Bengali text to Path, Curve and Mesh

Black and white image can be converted to mesh by first using Inkscape to trace the image bitmap, then saving as plain svg. Which can be imported into blender and conveted to mesh. Blender `2.92` contain Grease Pencil tracing which works for image masks too. Drag and drop image in blender then using `Object > Trace Image to Grease Pencil` will convert to grease pencil strokes which can converted to path, curve, mesh. The mesh can be extruded to create 3D text object.

Further details here,

https://stackoverflow.com/questions/67361964/blender-render-complex-fonts-as-rendered-with-python-pillow-libraqm/67367610#67367610

<br>

## TP Link Tapo C310 Wifi IP Camera Setup without Wired Internet on Router

I am not sure if internet on router is necessary to setup the camera with router first time. It can be used without internet on the router after setup. Admin password on router should be 16 characters or less. There is a problem of auto truncating a longer password on certain router models which can be missed easily causing login problem.

If wired internet is not available mobile hospot can be used to get internet in the router. If router has WDS bridge then it can be used to connect to mobile hotspot on 2.4GHz or 5GHz with password. This will allow other devices connected to router to get internet access of the mobile hotspot.

This camera has two setup process wifi or ethernet. Setup camera through Tapo mobile app by connecting to camera first and connecting to router after camera is connected to it. When using wifi camera will mention connected to wifi through loud speaker. 

`rtsp` streaming can be done via methods below for high and low resolution feed,

```
rtsp://username:password@camera_ip:554/stream1
rtsp://username:password@camera_ip:554/stream2
```

This can used to stream via VLC or `OpenCV video capture` to run deep learning models. Username and password needs to be setup via tapo app for above to work.

<br>

## Running Object Detection/Deep Learning Models Real Time on CPU

- Try INT8, FP16 quantization.
- Use ONNX runtime, OpenCV DNN, TFLite.
- Use model compression, prunning.
- Run drawing, other operations on separate threads.
- In OpenCV use threaded non blocking webcam, file video capture.   
- Detect in a frame and in next N frames run tracking only then again detect. Frame dropping and skipping.
- Empty frame buffer/queue to always get the latest frame.
- Do cpu specific compilation of code take advantage of cpu architectures such as AVX2, AVX512 etc.
- Compile with oneDNN, oneMKL, CUDA, oneTBB, gstreamer etc.
- Use vectorized implementations instead of naive approaches. Try Numba jit for cpu/gpu, CuPy.
- Use C++ instead of python for whole code.
- Move slower part of python code to C++ and call as library.
- Use a smaller input resolution for the model at the cost of accuracy.
- Convert model to onnx, openvino optimized format.
- Use better data structures and algorithms, tweak code to speedup.

<br>

## AWS EC2 TF, Pytorch Jupyter Notebook Tunneling and Browser Access

First follow this video by Dr. Jeff Heaton heaton to get started with the process. 

https://www.youtube.com/watch?v=WNfFD1MSj44

Various AMI images available in aws marketplace one of which is `NVIDIA Deep Learning AMI`. Other interesting ones are `AWS Deep Learning AMI (Ubuntu 18.04)` and `AWS Deep Learning Base AMI (Ubuntu 18.04)`. These seem to have pytorch, tensorflow setup by default. Linux version may be newer in future. Links to these AMI,

https://aws.amazon.com/marketplace/pp/prodview-e7zxdqduz4cbs

https://aws.amazon.com/marketplace/pp/prodview-dxk3xpeg6znhm

https://aws.amazon.com/marketplace/pp/prodview-x5nivojpquy6y

AWS marketplace AMI with aws, nvidia deep learning containers,

https://aws.amazon.com/marketplace/search/results?searchTerms=deep+learning+containers&CREATOR=c568fe05-e33b-411c-b0ab-047218431da9%2Ce6a5002c-6dd0-4d1e-8196-0a1d1857229b&filters=CREATOR

`Nvidia NGC` website has various containers such Tensorflow, Pytorch. For pytorch, tensorflow the docker commands available here,

https://ngc.nvidia.com/catalog/containers/nvidia:pytorch

https://ngc.nvidia.com/catalog/containers/nvidia:tensorflow

https://ngc.nvidia.com/catalog

Release notes on AWS and Nvidia AMI,

https://docs.aws.amazon.com/dlami/latest/devguide/appendix-ami-release-notes.html

https://docs.nvidia.com/ngc/ngc-ami-release-notes/

More information on Deep Learning AMI and EC2 `P3` instance, 

https://docs.aws.amazon.com/dlami/latest/devguide/gs.html

https://docs.aws.amazon.com/dlami/latest/devguide/overview-conda.html

https://aws.amazon.com/ec2/instance-types/p3/

To setup python container first setup tunneling port, ppk, ip via putty. These commands can be found on NGC containers link above. 

In putty `hostname` set ip address of the instance. Convert `*.pem` to `*.ppk` via puttygen with passphrase. Set `*.ppk` to `connection > SSH > Auth > Private key file for authentication`. Set `connection > SSH > Tunnels` source port to `8888`, destination to `localhost:8888` and click add. Save session settings with a name.

From putty terminal first run,

```
docker pull nvcr.io/nvidia/pytorch:21.05-py3
```

Afterward run below. The pytorch container version may be different and should be adjusted in both places. Mounting, container directory setup is also available in above NGC container link which I did not try here.

```
docker run --gpus all -it --rm -p 8888:8888 nvcr.io/nvidia/pytorch:21.05-py3
```

Following, https://stackoverflow.com/a/51851298/1689698, using the command for jupyter notebook gives the token which is used run jupyter notebook.

```
jupyter notebook --ip 0.0.0.0 --port 8888 --no-browser --allow-root
```

This will give somethis like, `http://hostname:8888/?token=<ALPHANUMERTIC_TOKEN>`. In the browser running, `http://localhost:8888/?token=<ALPHANUMERTIC_TOKEN>` will give accesss to jupyter notebook. It may take some time to load in browser for the first time.


#### Running jupyter-lab

For running jupyter-lab on aws using putty follow above method but use modified commands below.

```
docker run --gpus all -it --rm -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes nvcr.io/nvidia/pytorch:21.05-py3
```

```
jupyter lab --ip 0.0.0.0 --port 8888 --no-browser --allow-root
```

For mouting local directory try this command, `docker run --gpus all -it --rm -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -v /home/ubuntu/data/ml:/data/ml nvcr.io/nvidia/pytorch:21.05-py3`, here, `-v local_dir:container_dir`.

Copy file from docker container to host via, `docker cp <CONTAINER_ID>:<FILE_TO_COPY_PATH> <HOST_PATH>`. For example, `docker cp 22d5ce7be75a:/path/to/weight.pt /home/ubuntu/`.

OpenCV `ImportError: libGL.so.1: cannot open shared object file: No such file or directory` is solved by `!pip install opencv-python-headless`.

This way multiple terminals can be opened and anyone with ip address, ppk file will be able to access and modify jupyter notebook.

<br>

## Accessing Kaggle Dataset in AWS Jupyter Lab

Get API token from kaggle account > API > Create New Token. This will provide `kaggle.json` file. Install kaggle api via pip, `!pip install kaggle`. Upload the json file and copy/move to `~/.kaggle/kaggle.json`, example, `cp /workspace/kaggle.json ~/.kaggle/kaggle.json`. 

Delete the file if uploaded to workspace. Run, `chmod 600 /root/.kaggle/kaggle.json.`

Download dataset following kaggle API. For example celebA dataset on kaggle can be downloaded by, `!kaggle datasets download -d jessicali9530/celeba-dataset`.

## Tools for HikVision IP Camera Configuration

SADP Tool - Set gateway, ip, password etc. 
IVMS 4200 - Video recording, tools, configurations.
