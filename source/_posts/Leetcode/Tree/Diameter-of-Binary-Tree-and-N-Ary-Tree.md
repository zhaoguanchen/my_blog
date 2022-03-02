---
title: Diameter of Binary Tree and N-Ary Tree
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

本篇包含二叉树直径相关的题目，包括：

1. 二叉树的直径
2. n叉树的直径

# Diameter of Binary Tree

Num: 543. Diameter of Binary Tree

Link: https://leetcode.com/problems/diameter-of-binary-tree/



## Desc

Given the `root` of a binary tree, return *the length of the **diameter** of the tree*.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```
Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].
```

**Example 2:**

```
Input: root = [1,2]
Output: 1
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-100 <= Node.val <= 100`



## Solution

### 1. 后序遍历

#### 思路

-  递归寻找左右子节点的直径

#### 解法 

```java
class Solution {

    private int max = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        helper(root);
        return max;
    }

    private int helper(TreeNode root) {
        if (root == null) {
            return 0;
        }

        int left = helper(root.left);
        int right = helper(root.right);

        int current = left + right;
        // 记录最长直径
        max = Math.max(max, current);
        // 返回当前节点左子和右子中较长的路径长度，提供给上一节点
        return Math.max(left, right) + 1;
    }
}
```

   

 

 

# Diameter of N-Ary Tree

Num:  1522. Diameter of N-Ary Tree

Link: https://leetcode.com/problems/diameter-of-n-ary-tree/



## Desc

Given a `root` of an [N-ary tree](https://leetcode.com/articles/introduction-to-n-ary-trees/), you need to compute the length of the diameter of the tree.

The diameter of an N-ary tree is the length of the **longest** path between any two nodes in the tree. This path may or may not pass through the root.

(*Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value.)*

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/07/19/sample_2_1897.png)

```
Input: root = [1,null,3,2,4,null,5,6]
Output: 3
Explanation: Diameter is shown in red color.
```

**Example 2:**

**![img](https://assets.leetcode.com/uploads/2020/07/19/sample_1_1897.png)**

```
Input: root = [1,null,2,null,3,4,null,5,null,6]
Output: 4
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2020/07/19/sample_3_1897.png)

```
Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: 7
```

 

**Constraints:**

- The depth of the n-ary tree is less than or equal to `1000`.
- The total number of nodes is between `[1, 104]`.



## Solution

### 1. 后序遍历

#### 思路

-  参考二叉树的最大直径解法，将分析左右子改为分析子节点list

#### 解法 

```java
class Solution {

    private int max = 0;

    public int diameter(NNode root) {
        helper(root);
        return max;
    }

    private int helper(NNode root) {
        if (root == null) {
            return 0;
        }
        List<Integer> length = new ArrayList<>();

      	// 遍历得到每个孩子的最大路径
        for (int i = 0; i < root.children.size(); i++) {
            int current = helper(root.children.get(i));
            length.add(current);
        }
				// 排序产生最大值
        length.sort(Comparator.comparing(Integer::intValue).reversed());
        int top1 = length.size() > 0 ? length.get(0) : 0;
        int top2 = length.size() > 1 ? length.get(1) : 0;
        int current = top1 + top2;
        // 记录最长直径
        max = Math.max(max, current);
        // 返回当前节点左子和右子中较长的路径长度，提供给上一节点
        return top1 + 1;

    }  
}
```

   

#### 分析

- 每次针对孩子节点list的排序会产生不必要的性能和空间消耗。



### 2. 优化

#### 思路

-  去掉length列表的排序，在for循环中确定top1和top2.

#### 解法 

```java
class Solution {

    private int max = 0;

    public int diameter(NNode root) {
        helper(root);
        return max;
    }

    private int helper(NNode root) {
        if (root == null) {
            return 0;
        }
      
        // 最大值
        int top1 = 0;
        // 次大值
        int top2 = 0;

        for (int i = 0; i < root.children.size(); i++) {
            int current = helper(root.children.get(i));
          	// 值得注意的是，更新top1的同时，也要更新top2
            if (current > top1) {
                top2 = top1;
                top1 = current;
            } else if (current > top2) {
                top2 = current;
            }
        }

        int current = top1 + top2;
        // 记录最长直径
        max = Math.max(max, current);
        // 返回当前节点左子和右子中较长的路径长度，提供给上一节点
        return top1 + 1;

    }
}
```

#### 分析

- 改良后，Runtime: 0 ms, faster than 100.00% of Java online submissions for Diameter of N-Ary Tree.

##  

## Summary

- 参照后序遍历架构，根据子节点结果推算当前节点，再回溯至父节点。
- 过程中的排序等操作可以优化，会有较大的效率提升。



## Reference

[手把手刷二叉树](https://labuladong.github.io/algo/2/18/21/)

