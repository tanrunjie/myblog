---
title: cpp生成代码及数据类型
date: 2021-12-14 10:58:56
tags: c++ primer
categories:
- c++
---

c++基本数据类型主要有*bool, char, int, float, double*。具体如下：

![basic data structure](/images/12_14_2.png)
![basic data structure2](/images/12_14_3.png)

另外，C++**生成代码的过程**是：1.程序员编写源代码 2.编译器正确翻译C++为目标代码 3.链接器将环境启动的代码以及引入库的代码与目标代码正确链接 4.最后才输出机器认识的可执行代码
![code procedure](/images/12_14_1.png)

整型提升(integral promotion): 计算表达式时，bool, char, unsigned char, signed char short会转换为int，即int为计算机最自然的类型，也是运算速度可能最快的。较小与较大运算也可能提升。


