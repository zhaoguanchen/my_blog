---
title: Balanced Binary Tree
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

# Balanced Binary Tree
link: https://leetcode.com/problems/balanced-binary-tree/



## Desc

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the left and right subtrees of *every* node differ in height by no more than 1.



 Example

```
Input: root = [3,9,20,null,null,15,7]
Output: true
```



## Solution

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    boolean res = true;
    public boolean isBalanced(TreeNode root) {
        traversal(root);
        return res;
    }
    
    public int traversal(TreeNode root){
        if(root == null) return 0;
        if(res == false) {
              return -1;
        }
        int left = traversal(root.left);
        int right = traversal(root.right);
        
        if(Math.abs(left - right) > 1) res = false;
        return Math.max(left,right) + 1;
    }
    
}

```

why do we need to judge res?

```java
	if(res == false) {
		  return -1;
	}
```

当我们已经知道res == false 的时候，可以直接返回，不用进行下一步递归调用。

