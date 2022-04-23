---
title: CMake入门
date: 2022-04-20 22:21:10
categories: 环境配置
tags:
  - Programming
  - 环境配置
---



# CMake入门

## 1. 简单配置CMakeLists.txt

>  指定最低版本:

```cmake
cmake_minimum_required(VERSION 3.20)
```

> 指定项目名字:

```cmake
project(opencv)
```

> 构建可执行文件:

```cmake
add_executable(opencv main.cpp)
```

> 添加C++11标准支持:

 ```cmake
set(CMAKE_CXX_STANDARD 11)
 ```

****

## 2. 复杂工程配置CMakeLists.txt

> 设置库的路径(以opencv为例):

```cmake
set(OpenCV_DIR "E:\\Programming\\opencv\\mingw_build\\install")
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")
```

> 寻找第三方库:

```cmake
find_pacakage(OpenCv REQUIRED)
```

> 添加头文件:

```cmake
include_directories(${OpenCV_INCLUDE_DIRS})
```

> 在添加的可执行文件后链接OpenCv库:

```cmake
target_link_libraries(main ${OpenCV_LIBS})
```

****

## 3. 其他操作

> 添加子目录:

```cmake
add_subdirectory(OpenCV_Learning)
```

