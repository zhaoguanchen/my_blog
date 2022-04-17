---
title: Path Sum
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

# Path Sum

Num: 112. Path Sum

Link: https://leetcode.com/problems/path-sum/



## Desc

Given the `root` of a binary tree and an integer `targetSum`, return `true` if the tree has a **root-to-leaf** path such that adding up all the values along the path equals `targetSum`.

A **leaf** is a node with no children.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum1.jpg)

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
Output: true
Explanation: The root-to-leaf path with the target sum is shown.
```

 

## Solution

### 1. BFS

##### 思路

- BFS层序遍历。
- 采用队列存储节点。
- 本层遍历完毕，将子节点加入队列。
- 自顶向下按层循环，记录当前层的深度。到达叶子节点时判断和是否为target。

##### 解法 

```java
class Solution {
  
    /**
     * BFS
     *
     * @param root
     * @param targetSum
     * @return
     */
    public boolean hasPathSumBFS(TreeNode root, int targetSum) {
        if (root == null) {
            return false;
        }
        Queue<TreeNode> nodeQueue = new LinkedList<>();
        Queue<Integer> sumQueue = new LinkedList<>();

        nodeQueue.add(root);
        sumQueue.add(0);
        while (!nodeQueue.isEmpty() && !sumQueue.isEmpty()) {
            TreeNode currentNode = nodeQueue.poll();
            Integer sumOfFather = sumQueue.poll();
            Integer currentSum = sumOfFather + currentNode.val;
            // 到达叶子节点
            if (currentNode.right == null && currentNode.left == null) {
                if (currentSum == targetSum) {
                    return true;
                }
            } else {
                if (currentNode.left != null) {
                    nodeQueue.add(currentNode.left);
                    sumQueue.add(currentSum);
                }
                if (currentNode.right != null) {
                    nodeQueue.add(currentNode.right);
                    sumQueue.add(currentSum);
                }
            }
        }
        return false;
    }

}
```

 

### 2. DFS

##### 思路

- DFS递归遍历
- 到达叶子节点时，判断和是否为target



##### 解法  

```java
class Solution {

    /**
     * DFS
     *
     * @param root
     * @param targetSum
     * @return
     */
    private int target;

    public boolean hasPathSumDFS(TreeNode root, int targetSum) {
        if (root == null) {
            return false;
        }
        target = targetSum;
        return helper(root, 0);
    }

    private boolean helper(TreeNode root, Integer sumOfFather) {
        if (root == null) {
            return false;
        }
        int currentSum = sumOfFather + root.val;

        if (root.left == null && root.right == null) {
            return currentSum == target;
        }

        return helper(root.left, currentSum) || helper(root.right, currentSum);
    }

}

```

 

 

## Summary

本题与最大深度的题类似，区别在于记录的条件。

## Reference

[BFS 算法解题套路框架] https://labuladong.gitee.io/algo/1/6/