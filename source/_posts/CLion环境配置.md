---
title: CLion环境配置
date: 2022-04-20 22:17:16
categories: 环境配置
tags:
  - Programming
  - 环境配置
---

#  CLion环境配置

## 1. 准备

* CLion安装 https://account.jetbrains.com/
* [MinGW编译器(posix)](https://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release/)
* 镜像下载网址http://mirrors.163.com/cygwin/

***

## 2. 安装配置

### 2.1 安装包

* gcc-core
* gcc-g++
* cmake
* make
* gdb
* binutils

***

### 2.2 配置环境变量

将安装目录下mingw编译器的bin目录添加到path环境变量

![image-20220421224421487](https://cdn.jsdelivr.net/gh/roranrui/img_bed/img/image-20220421224421487.png)

***

###  2.3 配置CLion

![image-20220421224819432](https://cdn.jsdelivr.net/gh/roranrui/img_bed/img/image-20220421224819432.png)

配置完成后,检查是否可运行:

***

## 3.验证MinGw是否安装成功

cmd终端输入

```
gcc -v
```

出现说明即安装成功

***

## 4. 关于Cygwin 和MinGW 的区别与联系

> **参考文章** [https://www.zhihu.com/question/22137175](https://www.zhihu.com/question/22137175)

Cygwin的大部分架构源于Lunix, Cygwin 中既可以运行 Cygwin 的应用（依赖模拟层），又可以运行 Windows 应用

但用于Windows系统, `system("pause")`等调用系统的指令无法被识别

* Cygwin下替代`system("pause")`的指令

```cpp
cout << "按回车键后继续...";
cin.get();
```

MinGW 是用于进行 Windows 应用开发的 GNU 工具链(开发环境),它的编译产物一般是原生 Windows 应用,虽然它本身不一定非要运行在 Windows 系统下(是的 MinGW 工具链也存在于 Linux、BSD 甚至 Cygwin 下)。

但使用这个环境, 就会出现一个问题: **中文输出乱码**。MinGw 是基于 windows 运行的，新版 cmd 默认是 gbk 编码格式。当你的编码格式和默认编码格式不一样，这个乱码就开始了

* 解决:
  以下任意一行


```cpp
 system("chcp 65001");
 setlocale(LC_ALL,"zh-CN");
 SetConsoleOutputCP(65001);
```

