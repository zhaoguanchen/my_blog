---
title: Flatten Binary Tree to Linked List
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



本篇包含二叉树相关的题目，包括：

1.  拉平二叉树

# Flatten Binary Tree to Linked List

​	[114.  Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)



## Desc

Given the `root` of a binary tree, flatten the tree into a "linked list":

- The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
- The "linked list" should be in the same order as a [**pre-order** **traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR) of the binary tree.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

```
Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Example 3:**

```
Input: root = [0]
Output: [0]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-100 <= Node.val <= 100`

 

## Solution

### 1. 迭代

#### 思路

-  找到左子树最右节点，将右子树接到左子树最右节点
-  整个左子树作为右子树
-  沿着右子节点迭代

#### 解法 

```java
class Solution {

    public static void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
        TreeNode cur = root;

        while (cur != null) {
            if (cur.left != null) {
                // 找到左子树的最右节点
                TreeNode leftRight = cur.left;
                while (leftRight.right != null) {
                    leftRight = leftRight.right;
                }

                // 将右子树接到最右节点
                leftRight.right = cur.right;
                // 将左子树移到右边
                cur.right = cur.left;
                cur.left = null;
                // 完成后该层只有右子树
            }
            // 操作下一节点，也就是原左子节点
            cur = cur.right;
        }
    }

}
```

   

 

### 2. 递归

#### 思路

-  后序遍历，先处理子节点
-  找到左子树最右节点，将右子树接到左子树最右节点
-  整个左子树作为右子树

#### 解法 

```java
class Solution {

    public static void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
				// 递归左子树，右子树
        flatten(root.left);
        flatten(root.right);

      	// 元操作
        if (root.left != null) {
            // 找到左子树的最右节点
            TreeNode leftRight = root.left;
            while (leftRight.right != null) {
                leftRight = leftRight.right;
            }

            // 将右子树接到最右节点
            leftRight.right = root.right;
            // 将左子树移到右边
            root.right = root.left;
            root.left = null;
            // 完成后该层只有右子树
        }

    }

}
```

   

 

#### 

## Summary

- **写递归算法的关键是要明确函数的「定义」是什么，然后相信这个定义，利用这个定义推导最终结果，绝不要跳入递归的细节**。
- 根据某节点的操作推到递归或迭代的操作。



## Reference

[手把手刷二叉树第一期](https://labuladong.github.io/algo/2/18/22/)

