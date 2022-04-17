---
title: Find Duplicate Subtrees
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

 

本篇包含二叉树相关的题目，包括：

1. 寻找相同子树



以字符串的形式记录每个节点子树的结构，放入Map判断是否重复。

父节点结果由子节点推导，因此是后序遍历。

# Find Duplicate Subtrees

​	[652. Find Duplicate Subtrees](https://leetcode.com/problems/find-duplicate-subtrees/)



## Desc

Given the `root` of a binary tree, return all **duplicate subtrees**.

For each kind of duplicate subtrees, you only need to return the root node of any **one** of them.

Two trees are **duplicate** if they have the **same structure** with the **same node values**.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/08/16/e1.jpg)

```
Input: root = [1,2,3,4,null,2,4,null,null,4]
Output: [[2,4],[4]]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/08/16/e2.jpg)

```
Input: root = [2,1,1]
Output: [[1]]
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2020/08/16/e33.jpg)

```
Input: root = [2,2,2,3,null,3,null]
Output: [[2,3],[3]]
```

 

## Solution

### 1. 递归

#### 思路

-  以字符串形式记录节点子树的结构
-  通过map构建缓存，判断当前节点子树是否出现过。
-  父节点结构由左右子树得出，因此是后序遍历。

#### 解法 

```java
class Solution {


    private List<TreeNode> res;

    private Map<String, Integer> memo;


    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        res = new ArrayList<>();
        memo = new HashMap<>();
        helper(root);
        return res;
    }

    private String helper(TreeNode root) {
        if (root == null) {
            return "#";
        }

        String left = helper(root.left);
        String right = helper(root.right);

        String current = root.val + "/" + left + "/" + right;
        Integer count = memo.getOrDefault(current, 0);
        if (count == 1) {
            res.add(root);
        }

        memo.put(current, count + 1);
        return current;
    }

}
```



## Summary

- 分析题目，判断遍历方式（前序，中序，后序）。
- 参考二叉树的序列化，构造字符串。







## Reference

[手把手刷二叉树第三期](https://mp.weixin.qq.com/s/LJbpo49qppIeRs-FbgjsSQ)

