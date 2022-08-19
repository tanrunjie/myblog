---
title: C_Programming_Lang_第一部分
categories:
  - C 
mathjax: true
date: 2022-08-15 14:43:14
tags: C
---

### 课外小知识
常用的字符编码方式：
ASCII：表示纯英文，1个字节表示，第一位固定为0，其余七位表示128个字符。
扩展ASCII：表示欧洲文字的，1个字节表示，第一位也加入使用。
GBK：表示汉字。
Unicode：包含世界上所有字符的总集。
UTF-8：Unicode的实现之一，1-4个变长字节表示符号。

### Why C Important?
C is work for UNIX delevopment, all the UNIX systems and their softwares were programmed by C.

C provides lots of basic data structures: char, int, float; pointer, array, struct and union. Also provides basic logic structures: statements, if-else, switch, while/for, do break. Its function can return in reference, struct, union or pointer, even using in recursive.

During preprocess period, program text will be replaced by Macro  and compile in condition.

### 总览&ch1&ch2读书记录
new = 内存分配(malloc) + 构造函数
裸指针： 容易忘记delete， 指针容易到处传递，没法明确谁来执行释放。
智能指针shared_ptr: 引入计数器，创建为1，传参拷贝+1，对象析构则-1，变为0时自动释放。
智能指针相互引用问题：引入weaked_ptr，不增加引用计数。
智能指针兼容裸指针问题：重载了->和*运算符，用get()获得裸指针。

如果C源程序出现在多个文件中，那么对它的编译和装入要比将整个单一源做更多说明，但这是操作系统的任务，而非语言属性。

++操作只能用于变量，(i+j)++为非法。

位运算：&屏蔽某些位，|打开某些位，^异或，～取反， << >>移位