---
title: C++多线程实现
date: 2022-04-22 22:56:55
categories: 语言学习
tags:
  - Programming
  - C++
---

# 多线程实现

## 1. 线程运行的开始和结束

主线程从`main()`开始运行, 自己创建的线程也需要一个初始函数来运行

**整个进程是否执行完毕的标志是**: ==主线程是否执行完==

如果此时子线程还没执行完, 那么也会被系统强行终止

保持子线程的运行状态就需要**让==主线程保持运行==**

​	主线程从`main()`函数开始运行, 自己创建的线程也需要从**一个函数**开始运行



* 包含头文件 `<thread>`

示例代码

```cpp
#include <iostream>
#include <thread>
using namespace std;

void myPrint()
{
    cout << "我的线程开始了!!" << endl;
    cout << "我的线程执行完毕了!" << endl;
}


int main() {
    thread ts1(myPrint);
    ts1.join();  //  让主线程等待子线程执行完毕
    cout << "啊哈哈哈" << endl;
    return 0;
}

```

本程序有两个线程再跑, 有两条线**同时**再走, 不是顺序执行!!



## 2. Thread类

### 2.1 Thread类构造方法:

* 创建Thread对象，默认有一个线程名，以Thread-开头，按序增加的数字，从0开始
* 如果在构造Thread的时候没有传递Runnable或者没有复写Thread的run方法，该Thread不会执行任何东西。
* 如果构造Thread对象是没有传入ThreadGroup，则默认获取父线程的group作为当前线程的group。此时子线程和父线程在同一个group中
* stackSize可以指定线程的栈的大小，如果没有指定stackSize默认是0，0代表忽略该参数。该参数由JVM使用。注意：该参数有些平台无效，一般通过 -Xss 10M 设置栈的大小

**Thread类常用构造方法**:

* `Thread(函数)`
* `Thread(类对象(仿函数))`

```cpp
#include <iostream>
#include <thread>
using namespace std;

class TA
{
public:
    int my_i;
    TA(int &i)
    {
        my_i= i;
    }
    void operator()()
    {
        cout << "我的线程operator开始了!!" << endl;
        cout << "my_i = " << my_i << endl;
        cout << "my_i = " << my_i << endl;
    }

}

int main() {
    int myi = 999  // 主线程局部变量, 主线程执行完毕后, j
    TA ta(myi);
    thread ts1(ta);
    ts1.join();  //  让主线程等待子线程执行完毕
    cout << "啊哈哈哈" << endl;
    return 0;
}

```

`NOTE`: 创建线程时, 类对象是被复制到线程函数中去的, 且执行完毕后, 子线程复制的类对象会先被释放, 验证可用类的**拷贝构造函数, 析构函数**





****

### 2.2 join()方法

`join()` 方法的作用： **阻塞主线程**, 主线程等待当前子线程执行完成。还有两个重载方法：

* `join(long millis)` 等待指定的时间，单位毫秒
* `join(long millis, int nanos)` 和上面的方法类似，只是在毫秒的基础上在加了纳秒。更加精确

****

### 2.3 detach()方法

分离, 主线程不和子线程回合, 也就是主线程不必等待子线程执行完毕, 但**不太推荐**, `join()`方法即可

一旦detach之后, 与这个主线程关联的thread对象就会失去与主线程的关联, 此时这个子线程会驻留在后台运行, 由C++后台接管(**守护线程**), 也不能再用 `join`了, **不受控制**



示例代码:

```cpp
#include <iostream>
#include <thread>
using namespace std;

void myPrint2()
{
    cout << "我的线程开始了!!" << endl;
    cout << "我的线程执行完毕了!" << endl;
    cout << "我的线程执行完毕了!" << endl;
    cout << "我的线程执行完毕了!" << endl;
    cout << "我的线程执行完毕了!" << endl;
    cout << "我的线程执行完毕了!" << endl;
    cout << "我的线程执行完毕了!" << endl;
}

int main() {
    thread ts1(myPrint2);
    ts1.detach();
    cout << "啊哈哈哈" << endl;
    cout << "鸡汤来喽" << endl;
    cout << "啊哈哈哈" << endl;
    cout << "鸡汤来喽" << endl;
    cout << "啊哈哈哈" << endl;
    cout << "鸡汤来喽" << endl;
    return 0;

```

一旦主线程执行完毕, 就看不到子线程打印的信息了



## 2.4 joinable()

作用:

​	判断是否可以成功使用join()或者detach(),

返回: `true` 或 `false`

示例:

```cpp
#include <iostream>
#include <thread>
using namespace std;

void myPrint()
{
    cout << "我的线程开始了!!" << endl;
    cout << "我的线程执行完毕了!" << endl;
}


int main() {
    thread ts1(myPrint);
    ts1.join();  // 让主线程等待子线程执行完毕
    if (ts1.joinable())
    {
        cout << "啊哈哈哈" << endl;
    }
    else
    {
        cout << "鸡汤来喽" << endl;
    }
    return 0;
}

```

