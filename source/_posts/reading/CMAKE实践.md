---
title: CMAKE实践
categories:
  - 读书笔记
mathjax: true
date: 2022-02-25 16:49:17
tags: 读书笔记
---

### 初识cmake
主要特点：
- 1.开放源代码
- 2.跨平台，可生成native编译配置文件，在linux/unix平台生成makefile;在IOS平台生成xcode;在windows平台生成MSVC工程文件
- 3.可管理大型项目，如KDE4
- 4.简化编译构建过程和编译过程。工具链简单：cmake+make
- 5.高效率，cmake构建KDE4的kdelibs要比autotools快40%，因为cmake工具链中没有libtool
- 6.可拓展性，可以为cmake编写特定功能模块，扩展功能

缺点：
- 1.相对简单，但是没有听起来或者想象简单
- 2.cmake编写过程实际上是编程过程，使用的是cmake语言和语法
- 3.cmake与已有体系配合不算理想如pkgconfig

作者建议：
- 1.如果没有实际的项目需求，那么看到这里就可以停下来，因为cmake学习过程就是实践过程，没有实践读多少天也会忘记
- 2.如果你的工程只有几个文件，直接编写Makefile是最好的选择
- 3.如果使用c/c++/java以外的语言，请不要使用cmake
- 4.如果使用的语言有非常完备的构建体系，如java的ant，也不需要学习cmake
- 5.如果项目已经采用非常完备的工程管理工具，且不存在维护问题，没有必要迁移到cmake
- 6.如果仅仅使用qt，没必要使用cmake。因为qmake在QT的专业性和自动化程度更高

