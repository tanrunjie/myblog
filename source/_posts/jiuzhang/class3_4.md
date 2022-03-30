---
title: class3_4
categories:
  - 九章算法
mathjax: true
date: 2022-03-24 18:38:43
tags: 九章算法
---

### class 3
#### Binary Tree Traversal
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

### class 4
动态规划：利用已经计算的结果求解新的结果
