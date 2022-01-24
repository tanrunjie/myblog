---
title: EIGEN_1 HELLO_WOLRD
categories:
  - EIGEN
mathjax: true
date: 2022-01-20 15:19:05
tags: EIGEN
---

### EIGEN简介
FROM WIKI: Eigen is a high-level C++ library of template headers for linear algebra, matrix and vector operations, geometrical transformations, numerical solvers and related algorithms. 
简而言之C++开源模板库，支持线性代数、矩阵矢量运算和数值分析，是个方便好用的数学库。

更绝的是，EIGEN所有源码用头文件编写，所以只需要Include,无需编译链接过程，直接与平台无关，非常通用！

### 安装
安装过程也很简单，在UBUNTU下直接
``` cpp
sudo apt-get install libeigen3-dev
```
一般安装在/usr/include目录下

使用直接与标准库类似，#include <Eigen/Dense>等


### 主要模块
![](/images/01_20_1.png)

### 简单例子
``` cpp
#include <iostream>
#include <eigen3/Eigen/Dense>

using namespace std;
using namespace Eigen;

int main()
{

    Matrix3d m = Matrix3d::Random();
    m = ( m + Matrix3d::Constant(1.2)) * 50;

    cout << m << endl;
    Vector3d v(3);
    v << 1, 2, 3;
    cout <<m*v << endl;
    
    return 0;
}

```
这是fix_sized定义的写法，一般4*4以下的矩阵矢量用固定大小写法，对于比较大的才用不定量写法MatirxXd, VectorXd;

主要结构为1D对象Vector, Array和2D对象Matrix