---
title: Binary Tree Zigzag Level Order Traversal
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

# Binary Tree Zigzag Level Order Traversal

Num: 103. Binary Tree Zigzag Level Order Traversal

Link: https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/



## Desc

Given the `root` of a binary tree, return *the zigzag level order traversal of its nodes' values*. (i.e., from left to right, then right to left for the next level and alternate between).

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]
```

 

## Solution

### 1. BFS

##### 思路

- 改进常规的层序遍历算法，依靠LinkedList的特性根据遍历方向控制节点值插入位置（头部/尾部）。

##### 解法 

```java
class Solution {
  
    /**
     * 常规BFS
     * 层序遍历改进
     *
     * @param root
     * @return
     */
    public static List<List<Integer>> zigzagLevelOrderNormal(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        // 当前层是否从左到右， 以此判断该层节点的添加顺序
        boolean currentOrder = true;
        while (!queue.isEmpty()) {
            int size = queue.size();
            LinkedList<Integer> levelList = new LinkedList<>();
            for (int i = 0; i < size; i++) {
                TreeNode currentNode = queue.poll();
                if (currentOrder) {
                    levelList.add(currentNode.val);
                } else {
                    levelList.addFirst(currentNode.val);
                }
                if (currentNode.left != null) {
                    queue.add(currentNode.left);
                }
                if (currentNode.right != null) {
                    queue.add(currentNode.right);
                }

            }
            currentOrder = !currentOrder;
            result.add(levelList);
        }
        return result;
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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Deque<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        // 当前层是否从左到右
        boolean currentOrder = true;
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> levelList = new ArrayList<>();
            Deque<TreeNode> temp = new LinkedList<>();
            for (int i = 0; i < size; i++) {
                TreeNode currentNode = queue.pollLast();
                levelList.add(currentNode.val);
                if (currentOrder) {
                    if (currentNode.left != null) {
                        temp.add(currentNode.left);
                    }
                    if (currentNode.right != null) {
                        temp.add(currentNode.right);
                    }
                } else {
                    if (currentNode.right != null) {
                        temp.add(currentNode.right);
                    }
                    if (currentNode.left != null) {
                        temp.add(currentNode.left);
                    }
                }
                
            }

            queue.addAll(temp);
            result.add(levelList);
            currentOrder = !currentOrder;
        }
        return result;
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



## Reference

[BFS 算法解题套路框架] https://labuladong.gitee.io/algo/1/6/