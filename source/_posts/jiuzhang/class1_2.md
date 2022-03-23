---
title: class1_2
categories:
  - 九章算法
mathjax: true
date: 2022-03-17 15:03:52
tags: 九章算法
---

### 第一课

#### strStr问题
常见问题： 上来直接说KMP算法（过难）
可以确认面试的考察点，比如时间复杂度和空间复杂度

``` java
class Solution{
  /*
    return a index to first occurence of target in source, or -1 if not exist
  */
  public int strStr(String s1, String s2){
    if(s1==null || s2==null) return -1;
    int j;
    for(int i=0; i<s1.length()-s2.length()+1;i++)
    {
      for(j=0; j<s2.length();j++)
        if(s1.charAt(i+j)!=s2.charAt(j)) break;
      if(j==s2.length()) return i;
    }
  return -1;
  }
}

```

面试误区：
- 做过的题肯定能过
- 算法想出来就能过
- 代码写出来就能过

面试官眼中的求职者：可能是未来的同事
- 你的代码看起来舒服吗？需要多少时间REVIEW
- 你的Coding习惯好吗？不会未来疲于帮你debug，动不动搞挂网站
- 你的沟通能力好吗？和你交流费劲吗

主要考察编程的基本功：
- 程序风格（缩进，括号，变量名）
- Coding习惯（异常检查，边界处理）
- 沟通（让面试官时刻明白你的意图）
- 测试（主动写出合理的TestCase)

面试技巧：
- 在白纸能写吗
- 刷过的题，吃透了多少

明确一点：算法，其实很简单。类似于高考做题，刷题需要总结
- 归类相似的题目
- 找出适合的模板程序
- 掌握递归算法，掌握区分度


### 第二课

#### 二分搜索通用模板
``` cpp
int binarySearch(vector<int> &A, int target)
{
  if(A.size()==0)
    return -1;
  
  int start  = 0;
  int end = A.size()-1;
  int mid;

  while(start+1 < end)
  {
    mid = start + (end-start)/ 2;
    if(A[mid] == target)
      end = mid;  // 为准确找到第一个出现的target
    else if(A[mid] <target)
      start = mid;
    else
      end = mid;
  }
  if(A[start] == target)
    return start;
  if(A[end] == target)
    return end;
  
  return -1;
}

```

相关题目：二分查找first order/ last order, 二分查找区域， 二分插入数值， 增长矩阵二分查找等
