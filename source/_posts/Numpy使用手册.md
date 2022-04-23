---
title: Numpy使用手册
date: 2022-04-22 23:48:16
categories: 语言学习
tags:
  - Python
  - Programming
---

# Numpy使用手册

## 1. 一维数组

NumPy Ndarray 对象
NumPy 最重要的一个特点是其 N 维数组对象 ndarray，它是一系列同类型数据的集合，以 0 下标为开始进行集合中元素的索引。

ndarray 对象是用于存放同类型元素的多维数组。

ndarray 中的每个元素在内存中都有相同存储大小的区域。

ndarray 内部由以下内容组成：

一个指向数据（内存或内存映射文件中的一块数据）的指针。

数据类型或 dtype，描述在数组中的固定大小值的格子。

一个表示数组形状（shape）的元组，表示各维度大小的元组。

一个跨度元组（stride），其中的整数指的是为了前进到当前维度下一个元素需要"跨过"的字节数。

ndarray 的内部结构:

![img](https://cdn.jsdelivr.net/gh/roranrui/img_bed/img/pREJuYgw19sKbjn.png)

跨度可以是负数，这样会使数组在内存中后向移动，切片中 obj[::-1] 或 obj[:,::-1] 就是如此。

创建一个 ndarray 只需调用 NumPy 的 array 函数即可

***

#### 1.1 创建一维数组

API: `np.array(obj)`, obj 的类型只可以是列表或者元组

实例:

```python
import numpy as np 
a = np.array([1,2,3])  
print (a)
```

结果为:

```python
[1 2 3] 
```

***

#### 1.2 数组的数据类型

| bool_      | 布尔型数据类型（True 或者 False）                            |
| ---------- | ------------------------------------------------------------ |
| int_       | 默认的整数类型（类似于 C 语言中的 long，int32 或 int64）     |
| intc       | 与 C 的 int 类型一样，一般是 int32 或 int 64                 |
| intp       | 用于索引的整数类型（类似于 C 的 ssize_t，一般情况下仍然是 int32 或 int64） |
| int8       | 字节（-128 to 127）                                          |
| int16      | 整数（-32768 to 32767）                                      |
| int32      | 整数（-2147483648 to 2147483647）                            |
| int64      | 整数（-9223372036854775808 to 9223372036854775807）          |
| uint8      | 无符号整数（0 to 255）                                       |
| uint16     | 无符号整数（0 to 65535）                                     |
| uint32     | 无符号整数（0 to 4294967295）                                |
| uint64     | 无符号整数（0 to 18446744073709551615）                      |
| float_     | float64 类型的简写                                           |
| float16    | 半精度浮点数，包括：1 个符号位，5 个指数位，10 个尾数位      |
| float32    | 单精度浮点数，包括：1 个符号位，8 个指数位，23 个尾数位      |
| float64    | 双精度浮点数，包括：1 个符号位，11 个指数位，52 个尾数位     |
| complex_   | complex128 类型的简写，即 128 位复数                         |
| complex64  | 复数，表示双 32 位浮点数（实数部分和虚数部分）               |
| complex128 | 复数，表示双 64 位浮点数（实数部分和虚数部分）               |

指定数据类型 --`dtype`

* `dtype = np.int32`  -- 直接指定名称
* `dtype = 'i4'`  -- 使用字符串代号(**int8, int16, int32, int64** 四种数据类型可以使用字符串 **'i1', 'i2','i4','i8'** 代替)

***

#### 1.3 一维数组的函数

API:

1. `np.arrange(start, stop, step, dtype)`创建数组
   * **start:** 开始值, 默认为0, **包含**开始值
   * **stop:** 结束值, **不包含**结束值
   * **step:** 步长, 默认1, 可为负

2. `np.linspase(start, stop, num, endpoint, restep, dtype)` 创建**等差**数组
   * **num:** 设置生成的元素个数
   * **endpoint:** 是否包含结束值, 默认True
   * **restop:** 是否返回步长(公差), 默认False(如果为True, 返回的是二维数组, 包括数组和步长)

3. `np.logspace(start, stop, num, endpoint, base, dtype)` 创建**等比**数组
   * **start:** 开始值 = base ** start
   * **stop:** 结束值 = base ** stop
   * **base:** 底数, 默认为0

实例: 

```python
import numpy as np
# %% 初始值0 ,结束值不为10, 步长1
a = np.arange(10)
print(a)

# %% 初始值1, 结束值不为10, 步长2
b = np.arange(start=1, stop=10, step=2)
print(b)

# %% 数据类型

c = np.arange(1, 10, 2, dtype=np.float64)
print(c)

# %% 等差数组
d = np.linspace(start=1, stop=99, num=20, endpoint=True, retstep=True, dtype='i4')
print(d)

# %% 等比数组
e = np.logspace(1, 101, num=10, endpoint=True, dtype=np.float64)
print(e)

```

***

## 2. 二维数组

**数组的轴**

二维数组有两个轴, 轴索引分别为0和1

![image-20211123224715748](https://cdn.jsdelivr.net/gh/roranrui/img_bed/img/JwoVU1W7cjsGDYv.png)

**数组转置: **利用数组对象T, 将轴索引交换

***

#### 2.1 二维数组的函数

API:

1. `np.ones(shape, dtype=None)`创建全为**一**的数组
   * **shape:** 数组形状(可为列表, 元组) [2, 3]二行三列

2. `np.zeros(shape, dtype=None)`创建全为**零**的数组
3. `np.full(shape, fill_value, dtype=None)`根据指定形状和类型生成数组, 并用指定数据填充
   * **fill_value:** 指定填充的数据
4. `np.identity(n, dtype=None)`创建单位矩阵(对角线元素为1, 其他元素为0的矩阵)
   * **n:** 数组形状

实例:

```python
import numpy as np
# %% 6x5 1
a = np.ones([6, 5])
print(a)

# %% 3x2 0
b = np.zeros([3, 2], dtype=np.float32)
print(b)

# %% 3x3 5
c = np.full([3, 3], fill_value=5, dtype=np.complex64)
print(c)

# %% 单位矩阵5x5
d = np.identity(5)
print(d)
```

***

## 3. 数组访问

### 3.1 索引访问

即通过对应下标访问元素

* 一维: `ndarrary[index]`;
* 二维: `ndarray[0轴索引][1轴索引]`;

实例:

```python
import numpy as np
# %% 一维
a = np.array([1, 5, 6, 7, 9])
b = a[3]
print(b)

# %% 二维
c = np.array([[2, 3, 4],
            [3, 4, 5],
            [7, 8, 9]])
d = c[1][1]
print(d)

```

***

### 3.2 切片访问

一维:

* `ndarray[start:end]`
* `ndarray[start:end:step]`
  * **step**: 步长, 默认1
  * 包含起始值**不包含结束值**

二维:

* `ndarray[所在0轴切片, 所在1轴切片]`
  * 切片的元素类型与原数据相同
  * 两个切片是二维数组
  * 一个切片一个标量是一维数组

实例:

```

```

```python
import numpy as np
# %% 一维数组索引
a = np.array([1, 4, 6, 7, 9, 10])
b = a[1:5]
print(b)

# %% 步长为2
c = np.array([1, 4, 6, 7, 9, 10])
d = c[1:6:2]
print(d)

# %% 二维数组
e = np.array([[1, 3, 5, 6],
             [2, 4, 6, 8],
             [3, 5, 6, 1]])
f = e[0:1, 1:3]
print(f)

# %% 切片中有一个是标量
h = np.array([[1, 3, 5, 6],
             [2, 4, 6, 8],
             [3, 5, 6, 1]])
i = h[0:2, 3]
print(i)
```

****

### 3.3 布尔索引

* 布尔索引必须与要索引的数组**形状相同**, 否则会引发IndexError错误
* 布尔索引返回的新数组是原数组的副本, 拥有独立的内存空间(切片索引为浅层复制)
* 为Ture 保留, 为False 剔除
* **布尔索引输出的值一定是一个一维数组**

实例:

```python
 import numpy as np
# %% 一维布尔索引
a = np.array([1, 4, 6, 7, 9])
b = np.array([True, False, True, False, True])
print(a[b])

# %% 二维布尔索引
c = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]])
d = np.array([[True, True, True],
              [False, False, False],
              [True, False, True]])
print(c[d])
```

****

### 3.4 花式索引

索引方法:

* 索引为整数列表
* 索引为一维整数数组
* 索引为二维整数数组
* 索引返回的新数组是原数组的副本, 拥有独立的内存空间
* 二维数组上**每一个轴**的索引数组形状相同

实例:

```python
import numpy as np
# %% 一维数组的花式索引(二维索引)
a = np.array([1, 2, 3, 4, 5, 6])
b = np.array([[1, 2],
              [3, 4]])
print(a[b])

# %% 一维数组的花式索引(一维索引)
c = np.array([1, 2, 3, 4, 5, 6])
d = np.array([1, 2, 3, 4])
print(c[d])

# %% 一维数组的花式索引(整数列表)
e = np.array([1, 2, 3, 4, 5, 6])
f = [1, 2, 3, 4]
print(e[f])

# %% 二维数组的花式索引(整数列表)
a1 = np.array([[1, 2, 3],
               [4, 5, 6],
               [7, 8, 9]])
m = [1, 2]
n = [0, 1]
print(a1[m][n])

# %% 二维数组的花式索引(一维索引)
a2 = np.array([[1, 2, 3],
               [4, 5, 6],
               [7, 8, 9]])

m = np.array([1, 2])
n = np.array([0, 1])
print(a2[m][n])

# %% 二维数组花式索引(二维数组)
a3 = np.array([[1, 2, 3],
               [4, 5, 6],
               [7, 8, 9]])

b3 = np.array([[1, 2],
               [0, 2]])
b4 = np.array([[0, 1],
               [1, 2]])
print(a3[b3, b4])
```

****

## 4. 数组处理

### 4.1 连接数组

API:

1. `np.concatenate((a1, a2, ...), axis)` 沿指定的轴连接多个数组
   * **(a1, a2, ...):** 要连接的数组
   * **axis:** 轴, 默认为0轴

2. `np.vstack((a1, a2, ...))` 沿垂直堆叠多个数组, 相当于concatenate() axis=0
   * **1轴元素个数要相同**

3. `np.hstack((a1, a2, ...))` 沿水平堆叠多个数组, 相当于concatenate() axis=1
   * **0轴元素个数要相同**

实例:

```python
import numpy as np
# %% 沿指定的轴连接多个数组
a = np.array([1, 2, 3, 4, 5])
b = np.array([2, 3, 4, 5, 6])

c = np.concatenate((a, b), 0)
print(c)

# %% 沿垂直堆叠多个数组
a1 = np.array([[1, 2, 3],
               [4, 5, 6],
               [7, 8, 9]])
b1 = np.array([[1, 2, 3],
               [4, 5, 6]])

c1 = np.vstack((a1, b1))
print(c1)

# %% 沿水平堆叠多个数组
a2 = np.array([[1, 2],
               [4, 5],
               [7, 8]])
b2 = np.array([[1, 2, 3],
               [4, 5, 6],
               [7, 8, 9]])
c2 = np.hstack((a2, b2))
print(c2)
```

****

### 4.2 分割数组

API:

1. `np.split(arr, indices_or_scetions, axis)`沿指定的轴分割成多个数组
   *  **arr:** 要分割的数组
   *  **indices_or_scetions:** 如果是**整数**就平均分割, 如果是**数组**, 则为沿指定的轴的切片操作
   *  **axis:** 默认0轴

2. `np.vsplit(arr, indices_or_scetions)`沿着垂直方向分割数组, 相当于split() axis=0
3. `np.hsplit(arr, indices_or_scetions)`沿着水平方向分割数组, 相当于split() axis=1

实例:

```python
import numpy as np
# %% 沿指定的轴分割成多个数组(整数)
a = np.array([[1, 2, 3, 5],
              [4, 5, 6, 3],
              [7, 8, 9, 8]])
b = np.split(a, 3)
print(b)

# %% 沿指定的轴分割成多个数组(数组)
a1 = np.array([[1, 2, 3, 5],
              [4, 5, 6, 3],
              [7, 8, 9, 8],
               [1, 5, 6, 8]])
sections = np.array([1, 2])

b1 = np.split(a1, sections)
print(b1)

# %% 沿着垂直方向分割数组
a2 = np.array([[1, 2, 3, 5],
              [4, 5, 6, 3],
              [7, 8, 9, 8]])
b2 = np.vsplit(a, 3)
print(b2)

# %% 沿着水平方向分割数组
a3 = np.array([[1, 2, 3, 5],
              [4, 5, 6, 3],
              [7, 8, 9, 8],
               [1, 5, 6, 8]])
sections1 = np.array([1, 2])
b3 = np.hsplit(a3, sections1)
print(b3)
```

****

### 4.3 数组的算数运算

数组的运算用**标准运算符**即可

实例:

```python
import numpy as np
# %% 一维
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

print(a ** b)

# %% 一维
a1 = np.array([1, 2, 3])
b1 = np.array([4, 5, 6])

print(a1 ** 2)

# %% 二维
a2 = np.array([[1, 2],
               [3, 4]])
b2 = np.array([[5, 6],
               [7, 8]])
print(a2 + b2)

# %% 二维
a2 = np.array([[1, 2],
               [3, 4]])
b2 = np.array([[5, 6],
               [7, 8]])
print(a2 * 3)

```

****

### 4.3 数组广播

​	当数组与标量或者不同形状的数组进行算术运算时, 就会发生数组广播

下面的图片展示了数组 b 如何通过广播来与数组 a 兼容。

![img](https://i.loli.net/2021/11/27/2PgsYGzu1DE8yVS.gif)

**广播的规则:**

* 先比较形状, 再比较维度, 最后比较对应轴长度
* 若两数组的纬度不相等, 会在较低维度数组的形状左侧填充1, 直到维度相等
* 两数组维度相等: 如果对应轴长度相等, 则兼容, 可以发生数组广播, 若其中一个轴长度为1, 长度为1的轴会拓展

**简单理解：**对两个数组，分别比较他们的每一个维度（若其中一个数组没有当前维度则忽略），满足：

* 数组拥有相同形状。
* 当前维度的值相等。
* 当前维度的值有一个是1。

实例:

```python
import numpy as np
# %% 数组与标量
a = np.array([1, 3, 5])

print(a + 2)  # [2, 2, 2]

# %% 数组与数组 一
a1 = np.array([1, 2])  # ([1, 2], [1, 2])
b1 = np.array([[3, 4], [5, 6]])

print(a1 + b1)

# %% 数组与数组 二
a2 = np.array([[1, 2]])  # (1, 2)  ([[1, 2], [1, 2]])
b2 = np.array([[3],
               [4]])  # (2, 1)  ([[3, 3], [3, 3]])
print(a2 + b2)

# %% 数组与数组 三
a3 = np.array([[1, 2]])  # ([[1, 2], [1, 2]])  (2, 2)
b3 = np.array([[3, 4, 5],
               [6, 7, 8]])  # (2, 3)
print(a3, b3)  # 无法计算, 不满足广播原则

```

****

## 5. 其他数组函数

### 5.1 随机数函数

API:

1. `np.random.rand(a0, a1, ...)` 返回[0.0, 1.0)的随机浮点数

2. `np.random.randint(low, high, size, dtype)` 返回[low, high)的随机整数

   * **size:** 数组形状

3. `np.random.normal(loc, scale, size)` 返回正态分布随机数

   * **loc:** 平均值
   * **scale:** 标准差

4. `np.random.randn(a0, a1, ...)` 返回标准正态分布随机数(loc=0, scale=1)

   实例:

   ```
   import numpy as np
   # %% 浮点数
   a = np.random.rand(3, 4)
   print(a)
   
   # %% 整数
   b = np.random.randint(4, 7, size=(8, 8))
   print(b)
   
   # %% 正态分布一
   c = np.random.normal(5, 3, (3, 4))
   print(c)
   
   # %% 正态分布二
   d = np.random.randn(3, 4)
   print(d)
   ```


****

### 5.2 排序函数

API: 

1. `np.sort(arr, axis=-1, kind='quicksort', order=None)` 按轴对数组进行排序
   * **arr: **要排序的数组
   * **axis:** 轴索引, 默认-1
   * **kind:** 排序类型(quicksort, mergesort, heapsort)
   * **order:** 排序字段
2. `np.asort(arr, axis=-1, kind='quicksort', order=None)` 对数组按轴进行排序索引, 即对索引排序

输出的是排好序的数在原数组中的位置

实例:

```python
import numpy as np
# %% 一维
a = np.random.randint(1, 99, (15,))
b = np.sort(a)
print(a)
print(b)

# %% 二维
a1 = np.random.randint(1, 99, (3, 4))
b1 = np.sort(a1, axis=0)
print(a1)
print(b1)

# %% 轴索引排序
c = np.random.randint(0, 10, size=(3, 4))
print(c)
d = np.argsort(c)
print(d)
```

****

### 5.3 聚合函数

#### 5.3.1 求和

API:

1. `np.sum(arr, axis=None)`

2. `np.nansum(arr, axis=None)` 忽略NaN

3. 使用数组对象的sum()方法:

   ``np.ndarray.sum(axis=None)`

实例:

```python
import numpy as np
# %% 求和
a = np.array([[1, 2],
              [3, np.nan]])

print(np.sum(a))
print((np.sum(a, axis=1)))
print(a.sum(axis=1))
print(np.nansum(a))
```

****

#### 5.3.2 求最大值

API:

1. `np.amax(arr, axis=None)`

2. `np.namax(arr, axis=None)` 忽略NaN

3. 使用数组对象的max()方法:

   `np.ndarray.max(axis=None)`

实例:

```python
import numpy as np
# %% 求最大值
a1 = np.array([[1, 2],
              [3, np.nan]])

print(np.amax(a1))
print((np.amax(a1, axis=1)))
print(a1.max(axis=1))
print(np.nanmax(a1))
```

****

#### 5.3.3 求最小值

API:

1. `np.amin(arr, axis=None)`

2. `np.namin(arr, axis=None)`

3. 使用数组对象的min()方法:

   `np.ndarray.min(axis=None)`

实例:

```python
import numpy as np
# %% 求最小值
a2 = np.array([[1, 2],
              [3, np.nan]])

print(np.amin(a2))
print((np.amin(a2, axis=1)))
print(a2.min(axis=1))
print(np.nanmin(a2))
```

****

#### 5.3.4 求平均值

API:

1. `np.mean(arr, axis=None)`

2. `np.nmean(arr, axis=None)` 忽略NaN

3. 使用数组对象的mean()方法:

   `np.ndarray.mean(axis=None)`

4. `np.average(a, axis=None, weights=None)`

   * **weights:** 权重

实例:

```python
import numpy as np
# %% 求平均值
a3 = np.random.randint(0, 89, (3, 4))

print(a3)
print(np.mean(a3))
print((np.mean(a3, axis=1)))
print(a3.mean(axis=1))
print(np.nanmean(a3))

# %% 求加权平均值
a4 = np.array([[1, 2],
               [3, 4]])

print(np.mean(a4))
print(np.average(a4, weights=[[0.7, 0.1],
                              [0.1, 0.1]]))  # 精度不够
```

****

## 6. 数组的保存与读取

### 6.1 数组的保存

API:

1. `np.save(file, arr, allow_pickle=True, fix_imports=True)` 可以将一个数组保存至".npy"的二进制文件中
   * **file:** 文件路径
   * **arr:** 数组
   * **allow_pickle:** 是否允许用pickle保存数组对象
   * **fix_imports:** 是否允许在Python2中读取Python3的数据
2. `np.savez(file)` 可以将多个数组保存至".npz"的**未压缩**二进制文件中
   * **多个数组相当于保存在字典中, 要给每个数组指定键**
3. `np.savez_compressed(file)` 可以将多个数组保存至".npz"的**压缩**二进制文件中

实例:

```python
import numpy as np
# %% 一个数组
a = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]])
np.save('./array_save01', a)

# %% 多个数组
a1 = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]])
b = np.array([1, 2, 3, 4, 5, 6])

np.savez('./array_savez01', arr_a1=a1, arr_b=b)

# %% 多个数组压缩
a2 = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]])
b1 = np.array([1, 2, 3, 4, 5, 6])

np.savez_compressed('./array_savez02', arr_a1=a2, arr_b=b1)
```

****

### 6.2 数组的读取

API:

* `np.load(file, mmap_mode, allow_pickle, fix_imports)` 读取 .npy 和 .npz 文件中的数组
  * **mmap_mode:** 表示内存的映射模式, 即在读取较大的np数组时的模式, 默认None

实例:

```python
import numpy as np
# %%
a = np.load('array_save01.npy')
print(a)

# %%
b = np.load('array_savez01.npz')  # 返回一个字典
print(b['arr_a1'])
print(b['arr_b'])

# %% 
c = np.load('array_savez02.npz')
print(c['arr_a1'])
print(c['arr_b'])
```

