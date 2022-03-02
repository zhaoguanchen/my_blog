---
title: Invert Binary Tree
date: 
updated:
tags: Tree
categories: 
  - Leetcode
  - Tree
keywords: Tree
description:
top_img:
comments: true
cover:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:




---

**写递归算法的关键是要明确函数的「定义」是什么，然后相信这个定义，利用这个定义推导最终结果，绝不要跳入递归的细节**。



本篇包含二叉树直径相关的题目，包括：

1.  翻转二叉树

# Invert Binary Tree

​	[226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)



## Desc

Given the `root` of a binary tree, invert the tree, and return *its root*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

```
Input: root = [2,1,3]
Output: [2,3,1]
```

**Example 3:**

```
Input: root = []
Output: []
```

 



## Solution

### 1. 前序遍历

#### 思路

-  递归交换子节点

#### 解法 

```java
class Solution {

    public TreeNode invertTree(TreeNode root) {
        helper(root);
        return root;
    }

    private void helper(TreeNode root) {
        // 到达叶子节点，返回
        if (root == null) {
            return;
        }

        // 交换左右节点
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;

        // 递归子节点
        helper(root.left);
        helper(root.right);

    }
}
```

   

 

####  分析

- 除了前序遍历（交换操作放在递归前面），还可以后序遍历（交换操作放在递归后面）。

##  

## Summary

- **写递归算法的关键是要明确函数的「定义」是什么，然后相信这个定义，利用这个定义推导最终结果，绝不要跳入递归的细节**。
- 写二叉树的算法题，都是基于递归框架的，我们先要搞清楚 `root` 节点它自己要做什么，然后根据题目要求选择使用前序，中序，后续的递归框架。
- 二叉树题目的难点在于如何通过题目的要求思考出每一个节点需要做什么，这个只能通过多刷题进行练习了。



## Reference

[手把手刷二叉树第一期](https://labuladong.github.io/algo/2/18/22/)

