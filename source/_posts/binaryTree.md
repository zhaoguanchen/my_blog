---
title: Balanced Binary Tree
date: 
updated:
tags: Tree
categories: Leetcode
keywords: domain
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

110. Balanced Binary Tree
link: https://leetcode.com/problems/balanced-binary-tree/

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



## What else?
Iterator
