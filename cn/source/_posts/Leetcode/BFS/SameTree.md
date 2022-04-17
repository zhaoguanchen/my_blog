---
title: SameTree
date: 
updated:
tags: BFS
categories: Leetcode
keywords: BFS, SameTree
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

# SameTree

Num: 100. Same Tree

Link: https://leetcode.com/problems/same-tree/

Symmetric Tree类似。

## Desc

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

**Example 1:**

```
Input: p = [1,2,3], q = [1,2,3]
Output: true
```

 

## Solution

### 1. 递归

##### 思路

由根节点递归至子节点。

 

```java
class Solution {
  
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        }

        if (p == null || q == null || p.val != q.val) {
            return false;
        }

        return isSameTree(p.right, q.right) && isSameTree(p.left, q.left);
    }

}
```



 

### 2. BFS

##### 思路

- 采用队列存储节点
- 本层遍历完毕，将子节点加入队列。

```java
// 计算从起点 start 到终点 target 的最近距离
int BFS(Node start, Node target) {
    Queue<Node> q; // 核心数据结构
    Set<Node> visited; // 避免走回头路
    
    q.offer(start); // 将起点加入队列
    visited.add(start);
    int step = 0; // 记录扩散的步数

    while (q not empty) {
        int sz = q.size();
        /* 将当前队列中的所有节点向四周扩散 */
        for (int i = 0; i < sz; i++) {
            Node cur = q.poll();
            /* 划重点：这里判断是否到达终点 */
            if (cur is target)
                return step;
            /* 将 cur 的相邻节点加入队列 */
            for (Node x : cur.adj()) {
                if (x not in visited) {
                    q.offer(x);
                    visited.add(x);
                }
            }
        }
        /* 划重点：更新步数在这里 */
        step++;
    }
}
```



 

```java
class Solution {

    public boolean isSameTree(TreeNode p, TreeNode q) {
        Deque<TreeNode> pDeque = new ArrayDeque<>();
        Deque<TreeNode> qDeque = new ArrayDeque<>();

        if (p == null && q == null) {
            return true;
        }

        if (p == null || q == null || p.val != q.val) {
            return false;
        }

        pDeque.add(p);
        qDeque.add(q);

        while (!pDeque.isEmpty() && !qDeque.isEmpty()) {
            TreeNode curP = pDeque.removeFirst();
            TreeNode curQ = qDeque.removeFirst();

            if (curP.val != curQ.val) {
                return false;
            }

            if (curP.left == null || curQ.left == null) {
                if (curP.left != null || curQ.left != null) {
                    return false;
                }
            } else {
                pDeque.add(curP.left);
                qDeque.add(curQ.left);
            }

            if (curP.right == null || curQ.right == null) {
                if (curP.right != null || curQ.right != null) {
                    return false;
                }
            } else {
                pDeque.add(curP.right);
                qDeque.add(curQ.right);
            }

        }

        return pDeque.isEmpty() && qDeque.isEmpty();
    }

}
```

 

## Reference

[BFS 算法解题套路框架] https://labuladong.gitee.io/algo/1/6/