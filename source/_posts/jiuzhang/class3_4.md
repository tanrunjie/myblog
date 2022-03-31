---
title: 九章基础class_3_4
categories:
  - 九章算法
mathjax: true
date: 2022-03-24 18:38:43
tags: 九章算法
---

### 第三课 分治
先序： 根 左 右
后序： 左 右 根
中序： 左 根 右

遍历方法：
- 递归方法：一句话描述递归函数，如把根节点放入res里；
- 优化成非递归：借助栈的数据结构，动手背诵；
- 分治，分解攻克：问题分成两半，或者多份，分别解决再合并答案；带return value的递归方法; 二叉树的通用解法

递归方法：退出条件，节点为空如何处理；问题分成左右两半怎么处理；分治方法递归一般带相同返回类型参数。

``` cpp
// 递归法：
    vector<int> preorderTraversal(TreeNode *root) {
        // write your code here
        vector<int> res;
        preorder(root, res);
        return res;
    }
     // 递归方法：把根节点放入res里
     void preorder(TreeNode* root, vector<int>& res)
     {
         if(root == nullptr)
         {
             return ;
         }
         res.push_back(root->val);
         preorder(root->left, res);
         preorder(root->right, res);
     }

// 非递归法
// 分治法
vector<int> preorderTraversal(TreeNode *root) {
        // write your code here
        vector<int> res;
        if(root == nullptr)
            return res;
        
        // divide
        vector<int> left = preorderTraversal(root->left);
        vector<int> right = preorderTraversal(root->right);

        // conquer
        res.push_back(root->val);
        res.insert(res.end(), left.begin(), left.end());
        res.insert(res.end(), right.begin(), right.end());

        return res;
    }
```

DFS分治法解决归并排序，快速排序以及大部分二叉树问题。
树型分析法来分析复杂度:如归并排序为O(nlogn),logn层，每层合并需遍历每个元素n,O(1)分，O（n)治，需要额外O（N）空间，保证稳定排序；快速排序一样为O(nlogn),用O（n)分，O(1)治，不是稳定排序。一个先局部后整体，一个先整体后局部的分治方法。  

#### BFS广度优先搜索
逐层执行的实现方法：
- 两个queue: 导来导去
- 一个queue+dummy node:加入层分割符
- 单queue(best)： 一层一层往下走

``` cpp 
vector<vector<int>> levelOrder(TreeNode *root) {
        // write your code here 基本功：唯手熟耳
        vector<vector<int>> res;

        if(root == nullptr)
            return res;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty())
        {
            vector<int> level;
            int size = q.size();  // 提前取size,因为q.size()会变化
            for(int i=0 ; i< size; i++)
            {
                TreeNode* head = q.front();
                q.pop();
                level.push_back(head->val);
                if(head->left != nullptr)
                    q.push(head->left);
                if(head->right != nullptr)
                    q.push(head->right);
            }
            res.push_back(level);
        }
        return res;
    }
```

要求掌握：
- 递归：遍历和分治
- 非递归实现： 前序和中序
- 利用queue的BFS实现

### 第四课 DP
动态规划：利用已经计算的结果求解新的结果
结合分治方法，带脑子搜索；脑子可以是二维数组，也可以是哈希表，的记忆化搜索。动规本质解决了重复计算的搜索。

动规的问题解法：1.递归 2.分治 3.记忆搜索 4.循环求解 5.自底向上（逆向） 6.自顶向下（正向）

动规常见题目状态类型：
- 矩阵DP （10%）
- 序列（40%)
- 双序列DP（40%）
- Backpack(10%)

极有可能是动规的题目条件：
- 1.Maximum/Minimum
- 2.Yes/No
- 3.Count(*)

极不可能为动规：
- 求出所有"具体"的方案，而非“个数”
- 输入是无顺序的“集合”而非“序列”

**DP四要素：**
- 状态 State: 存储小规模问题的结果
- 方程 Function: 状态联系，小的状态计算大的状态
- 初始化 Initialization: 最小的状态，起点
- 答案 Answer: 最大的状态，终点

