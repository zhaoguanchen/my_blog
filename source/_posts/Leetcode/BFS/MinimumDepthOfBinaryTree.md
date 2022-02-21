---
title: Minimum Depth of Binary Tree
date: 
updated:
tags: BFS, DFS
categories: Leetcode/BFS
keywords: BFS, DFS
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

# Minimum Depth of Binary Tree

Num: 111. Minimum Depth of Binary Tree

Link: https://leetcode.com/problems/minimum-depth-of-binary-tree/



## Desc

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

 **Example 1:**

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

**Example 2:**

```
Input: root = [1,null,2]
Output: 2
```

 

## Solution

### 1. BFS

##### 思路

- BFS层序遍历。
- 采用队列存储节点
- 本层遍历完毕，将子节点加入队列。
- 自顶向下按层循环，记录当前层的深度。遇到第一个叶子节点时，其深度即为最小深度。

##### 解法 

```java
class Solution {
  
    /**
     * BFS
     */
    public int minDepth(TreeNode root) {
        int depth = 0;
        if (root == null) {
            return depth;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            depth++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode currentNode = queue.poll();
              	// 找到叶子节点，第一个叶子节点的深度即为答案。
                if (currentNode.left == null && currentNode.right == null) {
                    return depth;
                }

                if (currentNode.left != null) {
                    queue.add(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.add(currentNode.right);
                }
            }
        }
        return depth;
    }
}
```

 

### 2. DFS

##### 思路

- DFS递归遍历
- 到达叶子节点时，比较记录最大深度



##### 解法  

```java
class Solution {

    /**
     * DFS
     *
     * @param root
     * @return
     */
    private int minDepth = Integer.MAX_VALUE;

    public int minDepthDFS(TreeNode root) {
        if (root == null) {
            return 0;
        }
        helper(root, 1);
        return minDepth;
    }

    private void helper(TreeNode root, int depth) {
        if (root == null) {
            return;
        }
        // 走到叶子节点，记录最大深度
        if (root.left == null && root.right == null) {
            minDepth = Math.min(minDepth, depth);
            return;
        }
        // 深度加一
        depth++;
        // 子节点
        helper(root.left, depth);
        helper(root.right, depth);

    }

}

```

 

 

## Summary

本题与最大深度的题类似，区别在于记录的条件。

## Reference

[BFS 算法解题套路框架] https://labuladong.gitee.io/algo/1/6/