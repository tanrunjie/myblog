---
title: effective_cpp 3rd
categories:
  - null
mathjax: 读书笔记
date: 2022-04-21 15:56:35
tags: 读书笔记
---
### 导读
声明为explicit的构造函数更佳，避免不明确的自动隐式转换
对象使用“=”会自动调用copy构造函数
命名尽可能带有意义的缩写：lhs, rhs; pw,pa; rw,ra;等
关于线程的点：当C++受到全世界关注时，多线程程序还没诞生。因此C++的多线程更多考虑线程安全性。

内置类型pass-by-value高效，自定义类型pass-by-reference高效。

### chap1 Accustoming to C++
#### C++的四大组成部分：
- 原生C，C语言的区别在于没有：模板，异常及重载
- 面向对象C++特性：类、封装、继承、多态、虚函数
- 模板C++特性：泛型编程
- STL算法库
根据应用场合所需使用其中的主要成分。

#### const, enum, inline rather than #define
使用#define则预处理器解释，编译器不可见,因此也没有进符号表，可能导致编译器无法跟踪具体错误。
static char倾向用const string，类中的static int用enum {num=5}; enum hack

- 对于简单常量，使用const或者enum
- 对于#define函数宏，使用内联函数

#### 尽可能使用const
const char* p = pa; // const data
char* const p = pa; // const ptr
规律：星号左侧const为const data, 星号右侧const为const ptr

因此，下面f1和f2是一致的
``` cpp
void f1(const Widget* pw);
void f2(Widget const * pw);
```

const 迭代器不得改变指向，但是指向的内容值可以改动。
const return可以预防"无意义的赋值动作"错误。
const成员函数目的：1.让接口容易理解，指导是否可以改动对象内容 2.使操作const对象成为可能，因为改善C++程序效率根本方法是pass by reference-to-const

``` cpp
// non-const
char& operator[](int pos)
{ return text[pos];}

// const
const char& operator[](int pos) const
{ return text[pos];}
```
非const操作可读可写，const操作只针对const对象的读取；

bitwise const（C++）与Logical const的思考：
引入mutable成员变量使在const成员函数中依然可变，从而打破bitwise const，达到Logical const；
但mutable不能解决所有const难题，如指向的外部指针需要做边界检验、日志访问信息、完善性检验等，则在类内部产生一个争议的怪物。不利于代码重复、编译加快及维护。一个办法是non-const operator[]调用其const operator[]兄弟，减少重复代码，但是需要两次转型：一次将*this从原型转型为const &,从而避免递归调用non const版本自身；一次是在返回值移除const。从而用const operator[]实现出non-const版本。反向做法则带有风险：曾经承诺不改动的对象被改动了。因此const成员函数调用non-const函数是错误行为，因为non const可能改变成员。

总结：熟悉const在指针/迭代器上；在指针迭代器及引用指向的对象身上；在函数参数和返回类型上；在成员函数上的使用细则。
- 声明const帮助编译器识别错误用法，const可施加于任何作用域内的对象、函数参数、函数返回类型、函数本身。
- 编译器实施bitwise constness,但编程时应该使用logical constness
- const和non-const成员函数有着实质等价的实现时，可令non-const版本调用const版本避免代码重复。

#### 确定对象被使用前被初始化
构造函数总是使用成员初始化列表
static对象：声明周期从构造到程序结束，包括global对象，namespace作用域内的对象、class内、函数内、file作用域内声明为static的对象。
区别于static的stack,heap-based对象属于动态内存。

其中的问题c++对”定义在不同编译单元内的non-local static对象“初始化相对次序无法明确定义。因为决定初始化次序相当困难，常见形式多个编译单元的non-local static对象经有“模板隐式具现化 implicit template instantiations”形成。

解决方法：将每个non-local static对象搬到自己专属函数内，函数返回一个reference指向它所包含的对象。即singleton设计模式。

记住：
- 为内置型对象进行手工初始化，因为C++编译器不能保证初始化
- 构造函数使用成员初始化列表，且尽量确保次序一致
- 解决“跨编译单元的初始化次序”问题，用local static对象替换non-local static对象

### chap2 构造/析构/赋值运算

#### 了解C++默默编写及调用哪些函数
空类会自动声明一个default构造函数，copy构造函数,一个copy assignment操作符和析构函数

#### 若不想使用自动函数，则明确拒绝
一种做法将不想被自动调用的函数主动声明为private(iostream库做法，只有private声明);如果外部调用则编译器报错，如果member或friend函数调用则链接器报错。
更绝的做法专门为阻止copying动作设计一个基类，通过private继承，避免member/friend函数访问。

#### 为多态基类声明virtual析构函数
从而确保对象derived成分被销毁，避免资源泄漏及败坏数据结构。

virtual函数实现细节不重要，重要的是带来对象体积的增强，引入virtual table pointer.因此也不能滥用virtual析构

一般使用心得：只有当class内含有至少一个virtual函数时，才为它声明virtual析构函数

注意的是：不要企图继承string, STL容器作为多态基类，因为这些均是带有“non-virtual析构函数”的class

``` cpp
class AWOV{
  public:
    virtual ~AWOV() = 0;  // 声明pure virtual析构
};

AWOV::~AWOV(){} // 需要明确定义，因为有一系列调用动作
```

记住：
- 多态基类应该声明virtual析构函数，如果class带有任何virtual函数，则应该拥有一个virtual析构函数
- class设计不是作为基类或者多态基类使用，就不要声明virtual析构函数，因为增大对象的体积


#### 别让异常逃离析构函数
C++不禁止从析构抛出异常，但如果多个对象析构同时抛出多个异常，则容易导致不明确行为。解决方法：1.析构中抛出异常即abort程序 2.吞下异常，虽然覆盖了失败原因的重要信息，但有时候程序继续运行很重要 3.较佳策略重新设计接口，让client在普通函数中对抛出的异常作反应

#### 绝不在构造和析构过程调用virtual函数
因为这类调用从不下降至derived class

#### 令operator= 返回一个reference to *this
从而实现连锁形式,令外防止自我赋值问题
``` cpp
e.g.
class Widget{
public:
  Widget& operator=(const Widget& rhs)
  {
    if(this== &rhs)
      return *this;
    
    delete pb;
    pb = new Bitmap(*rhs.pb);
    return *this;
  }
};
```
记住：
- 确保自我赋值=和copy有良好行为
- 确定多个对象为同一对象时仍然正确

#### 复制对象勿忘每一个成分
- copying函数应该确保复制“对象所有成员变量”及“所有base class成分”
- 不要尝试以某copying函数实现另一个copying函数。可将共同功能放入第三个函数中，被两个copying函数调用

### 资源管理（C++重要问题）
主要原则：一旦使用了，将来必须还给系统。
资源包括：内存、文件描述器、互斥锁、图形界面的字型和笔刷、数据库连接以及网络sockets。

为了警惕调用create动态内存之后，delete的语句有可能被控制流略过，可靠的做法依赖C++的“析构函数自动调用机制”，将资源放进对象内。

记住：
- 防止资源泄漏，使用RAII对象
- 两个常用的RAII classes分别是tr1::shared_ptr和auto_ptr,
RAII（资源获取即初始化）:1.设计类封装资源 2.在构造函数初始化 3.在析构中销毁 4.使用时声明类

“当一个RAII对象被复制，该如何处理”：
- 一种是禁止复制，copying声明为private
- 对底层资源进行“引用计数法”，一并复制管理的资源

关于资源管理类
- APIs往往要求访问原始资源，所以每个RAII类应该提供一个“取得其所管理资源”的办法
- 对原始资源的访问可能由显式转换或隐式转换而得，一般而言显示转换较安全，但隐式转换对客户比较方便

#### new和delete采取相同形式
核心问题：被删除的指针所指是单一对象或者对象数组？

避免对数组形式左typedefs，容易引起new []和delete []一致性问题。由于STL的string和vector，因此C数组需求几乎为零。

应该以独立语句将newed对象存储于智能指针内，如果不如此，一旦异常抛出有可能导致难以觉察的资源泄漏。

### 设计与声明

#### 让接口容易正确使用，不易被误用
接口种类：function、class、template，都是客户与你的代码互动的方式。

理想上，如果客户企图使用某个接口却没有获得他所预期的行为，这个代码不该通过编译；如果通过编译，则API的作为就该是客户所想要的。

tricks:使用特定类型作为接口参数,使用shared_ptr强迫RAII

记住：
- 好的接口容易正确使用，应该在所有接口中努力达成这些性质
- “促进正确使用”的办法包括接口的一致性以及内置类型行为兼容
- “阻止误用”的办法包括建立新类型，限制类型上的操作，束缚对象值以及消除客户的资源管理责任
- shared_ptr支持定制型删除器，可防范DLL问题，可被用来自动解除互斥锁

#### 设计class犹如设计type
设计包括：重载函数和操作符、控制内存的分配和归还、定义对象的初始化和终结
设计规范的一些问题：
- 新type的对象应该如何被创建和销毁
- 对象初始化和赋值有什么差别
- 新type的对象如果被passed by value意味着什么
- 新type的“合法值”是什么
- 新type需要配合某个继承图系吗
- 新type需要什么样转换
- 什么样的操作符和函数对新type是合理的
- 什么样的标准函数应该驳回
- 谁该取用新type的成员
- 什么是新type的“未声明接口”
- 你的新type有多么一般化
- 你真的需要一个新type吗

记住：class的设计就是type的设计，定义之前确定你已经考虑过本条款覆盖的所有主题

#### 用pass-by-reference-to-const替换pass-by-value
Pass-by-value还可能引起切割问题，某些特化信息丢失。

记住：
- 对于内置类型以及STL迭代器和函数对象，使用pass-by-value比较适当
- 对于其他struct, type, class用pass-by-reference-to-const更高效