---
title: Binary Tree Level Order Traversal and II
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

# Binary Tree Level Order Traversal and II

Num: 102. Binary Tree Level Order Traversal 

Link: https://leetcode.com/problems/binary-tree-level-order-traversal/



Num: 107. Binary Tree Level Order Traversal II

Link: https://leetcode.com/problems/binary-tree-level-order-traversal-ii/



## Desc

Given the `root` of a binary tree, return *the level order traversal of its nodes' values*. (i.e., from left to right, level by level). 

 `Binary Tree Level Order Traversal `为自顶向下的二叉树层序遍历。

`Binary Tree Level Order Traversal II`为自底向上的二叉树层序遍历。



## 1. 自顶向下

### 1. BFS

##### 思路

- 层序遍历算法。

##### 解法 

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> levels = new ArrayList<List<Integer>>();
        if (root == null) return levels;

        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.add(root);
        int level = 0;
        while (!queue.isEmpty()) {
            // start the current level
            levels.add(new ArrayList<Integer>());

            // number of elements in the current level
            int level_length = queue.size();
            for (int i = 0; i < level_length; ++i) {
                TreeNode node = queue.remove();

                // fulfill the current level
                levels.get(level).add(node.val);

                // add child nodes of the current level
                // in the queue for the next level
                if (node.left != null) queue.add(node.left);
                if (node.right != null) queue.add(node.right);
            }
            // go to next level
            level++;
        }
        return levels;
    }
}
```



 

### 2. DFS

##### 思路

- 声明全局变量list存储结果
- DFS递归遍历，将当前节点添加至所属层级的子列表中。

##### 解法 

```java
class Solution {
    List<List<Integer>> levels = new ArrayList<List<Integer>>();

    public void helper(TreeNode node, int level) {
        // start the current level
        if (levels.size() == level) {
            levels.add(new ArrayList<Integer>());
        }

        // fulfil the current level
        levels.get(level).add(node.val);

        // process child nodes for the next level
        if (node.left != null)
            helper(node.left, level + 1);
        if (node.right != null)
            helper(node.right, level + 1);
    }

    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) return levels;
        helper(root, 0);
        return levels;
    }
}
```

 



### 3. DFS

##### 思路

- DFS遍历
- 按层级设置结果列表，根据节点所属层级在遍历过程中扩充子列表。
- 通过控制list的插入位置（开始或末尾）来控制顺序。



##### 解法  

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 * int val;
 * TreeNode left;
 * TreeNode right;
 * TreeNode() {}
 * TreeNode(int val) { this.val = val; }
 * TreeNode(int val, TreeNode left, TreeNode right) {
 * this.val = val;
 * this.left = left;
 * this.right = right;
 * }
 * }
 */
class Solution {

    private final List<List<Integer>> result = new LinkedList<>();

    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        if (root == null) {
            return result;
        }
        helper(root, 0);
        return result;
    }

    public void helper(TreeNode root, Integer level) {
        if (root == null) {
            return;
        }
        List<Integer> currentLevelList;
        
        // 不存在当前层级子列表，则添加
        if (result.size() <= level) {
            currentLevelList = new LinkedList<>();
            result.add(level, currentLevelList);
        } else {
            currentLevelList = result.get(level);
        }
        // 避免数组越界，当list为空时直接add; 其他情况，判断插入位置
        if (currentLevelList.size() == 0) {
            currentLevelList.add(root.val);
        } else {
            // 偶数层为正向，从左到右
            // 奇数层为负向，从右到左
            if (level % 2 == 0) {
                currentLevelList.add(root.val);
            } else {
                currentLevelList.add(0, root.val);
            }
        }
        helper(root.left, level + 1);
        helper(root.right, level + 1);
    }
}
```

## 2. 自底向上

### 1. 依靠LinkedList的特性形成倒序

- 子结果集形成后，插入结果集头部，这样最终的层级是倒叙的

```java
class Solution {

    public static List<List<Integer>> levelOrderBottom(TreeNode root) {
        // LinkedList便于往列表头部添加数据
        LinkedList<List<Integer>> result = new LinkedList<>();
        if (root == null) {
            return result;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            List<Integer> list = new ArrayList<>();
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode current = queue.poll();
                list.add(current.val);
                if (current.left != null) queue.add(current.left);
                if (current.right != null) queue.add(current.right);
            }
            // 将结果加入头部，形成倒序
            result.addFirst(list);
        }
        return result;

    }
  
}
```



### 2. Other

在常规层序遍历后，利用`Collections.reverse(levels);`反转结果list。





## Summary

- BFS：利用队列
- DFS：利用公共List存储结果
- 适时采用LinkedList控制元素的顺序

 

## Reference

[BFS 算法解题套路框架] https://labuladong.gitee.io/algo/1/6/