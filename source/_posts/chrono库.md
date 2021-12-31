---
title: chrono库
categories:
  - c++
mathjax: true
date: 2021-12-29 16:09:34
tags: c++
---
计时，常用于测试代码的运行时间和效率。

### 以往用法ctime

``` cpp
#include <ctime>
using namespace std;

clock_t start = clock();
func();
clock_t end = clock();
cout << "spend " << (double)(end-start)/CLOCKS_PER_SEC <<" second" << endl;  // 精确到毫秒
```


### chrono用法
``` cpp
#include <chrono>
using namespace std;
using namespace chrono;

auto start = system_clock::now();
func();
auto end = system_clock::now();
auto duration = duration_cast<microseconds>(end-start);
cout <<"spend " << double(duration.count()) * microseconds::period::num / microseconds::period::den << " second" << endl;

```
其中，auto为自动类型；除了system_clock,还可用steady_clock和high_resolution_clock; microseconds表示微妙，甚至还有nanoseconds纳秒；num和den表示计时单位的分子和分母。
