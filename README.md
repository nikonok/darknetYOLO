<!-- README.md -->
# **Yolo-Windows**
---
1.[How to compile](#how_to_compile)

2.[How to train (Pascal VOC data)](#how_to_train_voc)

3.[How to use ini file](#how_to_use_ini)

4.[How to use demo mode](#how_to_use_demo)
---
# Some info
---
This repository is forked from: (https://github.com/AlexeyAB/darknet)
[More details](http://pjreddie.com/darknet/yolo/)

Requires:
* **MS Visual Studio 2015 (v140)**: [link1](https://go.microsoft.com/fwlink/?LinkId=532606&clcid=0x409), [link2(ISO)](https://go.microsoft.com/fwlink/?LinkId=615448&clcid=0x409)
* **CUDA 8.0 for Windows x64**: https://developer.nvidia.com/cuda-downloads
* **OpenCV 2.4.9**: https://sourceforge.net/projects/opencvlibrary/files/opencv-win/2.4.9/opencv-2.4.9.exe/download
  - To compile without OpenCV - remove define OPENCV from: Visual Studio->Project->Properties->C/C++->Preprocessor
  - To compile with different OpenCV version - change in file yolo.c each string look like **#pragma comment(lib, "opencv_core249.lib")** from 249 to required version.
  - With OpenCV will show image or video detection in window and store result to: test_dnn_out.avi

## How to compile
<a name="how_to_copile"></a>

1. If you have MSVS 2015, CUDA 8.0 and OpenCV 2.4.9 (with paths: `C:\opencv_2.4.9\opencv\build\include` & `C:\opencv_2.4.9\opencv\build\x64\vc12\lib` or `vc14\lib`), then start MSVS, open `build\darknet\darknet.sln`, set **x64** and **Release**, and do the: Build -> Build darknet

  1.1. Find files `opencv_core249.dll`, `opencv_highgui249.dll` and `opencv_ffmpeg249_64.dll` in `C:\opencv_2.4.9\opencv\build\x64\vc12\bin` or `vc14\bin` and put it near with `darknet.exe`

2. If you have other version of CUDA (not 8.0) then open `build\darknet\darknet.vcxproj` by using Notepad, find 2 places with "CUDA 8.0" and change it to your CUDA-version, then do step 1

3. If you have other version of OpenCV 2.4.x (not 2.4.9) then you should change pathes after `\darknet.sln` is opened

  3.1 (right click on project) -> properties  -> C/C++ -> General -> Additional Include Directories
  
  3.2 (right click on project) -> properties  -> Linker -> General -> Additional Library Directories
  
  3.3 Open file: `\src\yolo.c` and change 3 lines to your OpenCV-version - `249` (for 2.4.9), `2413` (for 2.4.13), ... : 

    * `#pragma comment(lib, "opencv_core249.lib")`
    * `#pragma comment(lib, "opencv_imgproc249.lib")`
    * `#pragma comment(lib, "opencv_highgui249.lib")` 


4. If you have other version of OpenCV 3.x (not 2.4.x) then you should change many places in code by yourself.

5. If you want to build with CUDNN to speed up then:
      
    * download and install **cuDNN 5.1 for CUDA 8.0**: https://developer.nvidia.com/cudnn
      
    * add Windows system variable `cudnn` with path to CUDNN: https://hsto.org/files/a49/3dc/fc4/a493dcfc4bd34a1295fd15e0e2e01f26.jpg
      
    * open `\darknet.sln` -> (right click on project) -> properties  -> C/C++ -> Preprocessor -> Preprocessor Definitions, and add at the beginning of line: `CUDNN;

## How to train (Pascal VOC data)
<a name="how_to_train_voc"></a>
**Before you perform these steps place the file `darknet.exe` in the directory of the repository (where the folders as `3rdparty`, `data`, `scripts` and others is located), further this directory will be called `\darknet`**

1. Download pre-trained weights for the convolutional layers (76 MB): http://pjreddie.com/media/files/darknet19_448.conv.23 and put to the directory `darknet\`

2. Download The Pascal VOC Data and unpack it to directory `darknet\data\voc` will be created dir `build\darknet\x64\data\voc\VOCdevkit\`:
    * http://pjreddie.com/media/files/VOCtrainval_11-May-2012.tar
    * http://pjreddie.com/media/files/VOCtrainval_06-Nov-2007.tar
    * http://pjreddie.com/media/files/VOCtest_06-Nov-2007.tar
    
    2.1 Download file `voc_label.py` to dir `darknet\data\voc`: http://pjreddie.com/media/files/voc_label.py

3. Download and install Python for Windows: https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe

4. Run command: `python darknet\data\voc\voc_label.py` (to generate files: 2007_test.txt, 2007_train.txt, 2007_val.txt, 2012_train.txt, 2012_val.txt)

5. Run command: `type 2007_train.txt 2007_val.txt 2012_*.txt > train.txt`

6. Set `batch=64` and `subdivisions=64` in the file `yolo-voc.2.0.cfg`: [link](https://github.com/AlexeyAB/darknet/blob/master/build/darknet/x64/yolo-voc.cfg#L3)

7. Start training by using `train_voc.cmd` or by using the command line: `darknet.exe detector train data/voc.data yolo-voc.2.0.cfg darknet19_448.conv.23`

If required change pathes in the file `darknet\data\voc.data`

More information about training by the link: http://pjreddie.com/darknet/yolo/#train-voc

## How to use ini file
<a name="how_to_use_ini"></a>

The ini parser was taken from: https://github.com/benhoyt/inih

By default in programm used `initialization.ini`

## How to use demo mode
<a name="how_to_use_demo"></a>
**Before you perform these steps place the file `darknet.exe` in the directory of the repository (where the folders as `3rdparty`, `data`, `scripts` and others is located), further this directory will be called `\darknet`**

1. Edit the `initialization.ini` file to suit your requirements. By default will run demo mode with image processing from webcam.

2. Run command `darknet.exe parsing_file`.
