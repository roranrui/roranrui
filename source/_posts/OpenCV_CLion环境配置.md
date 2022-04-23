---
title: OpenCV在CLion下的环境配置
date: 2022-04-20 17:43:15
categories: 环境配置
tags: 
  - 环境配置
  - Programming
---

# OpenCv环境配置

## 1. 配置前的准备

* [OpenCv源码](https://opencv.org/releases/)

![image-20211220191844237](https://s2.loli.net/2021/12/20/VnxUYgGlpaTySNA.png)

* [OpenCV拓展库](https://github.com/opencv/opencv_contrib)

* [MinGW编译器(posix)](https://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release/)

* [CMake](https://cmake.org/download/)

![image-20211220191750473](https://s2.loli.net/2021/12/20/AnvfKMpHGIo5SuN.png)

* [git](https://git-scm.com/download/win)

![image-20211220192028646](https://s2.loli.net/2021/12/20/JTsiWQ9FcINyXAh.png)

****

## 2. 执行CMake

​	将OpenCv源码解压完后, 将拓展库==opencv_contrib==文件夹放到同目录下, 并创建一个==migw_build==的文件夹, 如下图所示

![image-20211220192228022](https://s2.loli.net/2021/12/20/OHo2qvb1ydRauV6.png)

完成后, 打开CMake-gui软件, 准备cmake.
先配置好源文件路径和编译输出文件夹的路径，分别如下：

![image-20211220192748002](https://s2.loli.net/2021/12/20/JKYeqo9SCNnyIh4.png)

然后，点击下面的config按钮，就会让你选择编译器，选择==Specify native compilers==, 填入编译器指定路径:

![image-20211220192905442](https://s2.loli.net/2021/12/20/qyD4EGkpR9oVNZJ.png)

点击finish之后，就开始cmake的config了，config完后，结果如下：

![image-20211220192924762](https://s2.loli.net/2021/12/20/FmzToHKYbkxsSGv.png)

出现Configuring done之后，需要再次点击一下config，让这片红色消失：![image-20211220193023639](https://s2.loli.net/2021/12/20/xUiFsvW7kwQIhHD.png)

再次Configuring done之后，就点击generate，开始生成makefile文件:

![image-20211220193004838](https://s2.loli.net/2021/12/20/YrF2diyxIn4LhSX.png)

在Configuring done的基础上看到了Generating done，说明generate完成了

添加扩展包的路径的方法如下，再cmake_gui的窗口中找到OPENCV_EXTRA_MODULES_PATH这一项，可以看到是空白的，我们将OpenCV_contrib的modules文件夹路径添加进来，如下所示：

![image-20211220193229291](https://s2.loli.net/2021/12/20/dHBiEvahRU7erAJ.png)

然后再次点击configure,跟之前一样，也会出现红色，然后再次点击configure,红色消失，说明config完成，
然后就点击generate,出现generating done 就说明Opencv3.2的扩展包也cmake完了，扩展包cmake的图片就不贴出来了，反正跟上面三幅图是一样的。

当扩展包的cmake也出现Generating done之后，我们就可以开始执行cmake生产的makefile文件了，即开始编译链接的工作

****

## 3. 编译makefile文件

在cmake的目标文件夹中，单击右键，选择Git Bash Here，前提是你要已经安装了git
然后就打开了git bash的命令行窗口, 输入指令mingw32-make -j8(以8个线程进行)，开始执行编译链接工作，如下所示：

![img](https://s2.loli.net/2021/12/20/aLZnviNMxEc2DVf.png)

然后需要苦苦等待，看着编译进度从0%开始一点一点往上加，如果出现100%不报错，那么就成功了，如下所示：

![image-20211220193425249](https://s2.loli.net/2021/12/20/9B6HZbSIjpXxTqf.png)

如果出现错误, 很正常想办法解决就彳亍, 结尾会总结我出现的错误以及解决方法

****

## 4. 配置环境变量

配置相关路径到**用户环境变量**:

![image-20211220193658690](https://s2.loli.net/2021/12/20/8BVG9cXesjQNf2R.png)

****

## 5. CMakeLists.txt的配置

![image-20211220193814066](https://s2.loli.net/2021/12/20/wWJEsc5NTmY8dKC.png)



****

## 6. 测试

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;

int main()
{
    Mat img = imread("E:\\Programming\\C++\\OpenCv\\A_Soul.jpg", (1920, 1080));

    imshow("A_Soul", img);
    waitKey();

    return 0;
}

```

输出结果如下:

![image-20211220194020709](https://s2.loli.net/2021/12/20/F6l4NiEyCULQIxW.png)

结束!

****

## 附录: 遇到的问题(本人遇到的)

### 1. **MinGW编译OpenCV到vs_version.rc.obj处出错**, **我愿称之为究极折磨** 

![image-20211220194307607](https://s2.loli.net/2021/12/20/lvyBfxcD8pPOzdH.png)

各种编译过程中总是出现各种模块的==vs_version.rc.obj==这个文件缺失的问题, 而且还非常规律(气死!)

解决: 自行生成==vs_version.rc.obj==文件

[折磨](折磨!!!.txt)

```
F:\ProgramFlies\C++\tdm64-gcc\bin\windres.exe E:\Programming\opencv\mingw_build\modules\xxxx\vs_version.rc -O coff E:\Programming\opencv\mingw_build\modules\xxxx\CMakeFiles\opencv_xxxx.dir\vs_version.rc.obj
```

第一个路径是编译器==windres.exe==的路径, 第二个路径是对应模块的路径

只需要把出错的模块名字替换==xxxx==, 复制到cmd中执行, 就能生成对应的文件

****

### 2. mingw编译OpenCV error: 'mutex' in namespace 'std' does not name a type

​	**解决: 在mingw构建工具链的“线程模型:win32”中不支持互斥。必须选择任何具有“线程模型:posix”的工具链。**
**选择 -posix的免安装包。作为编译器**

****

### 3. opencv报错Process finished with exit code -1073741515 (0xC0000135)

解决: 在配置中添加环境变量

![image-20211220195434564](https://s2.loli.net/2021/12/20/Jjkfg9iN2dC6eWY.png)

****

