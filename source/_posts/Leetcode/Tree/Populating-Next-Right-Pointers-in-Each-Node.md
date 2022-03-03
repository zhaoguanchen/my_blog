---
title: Populating Next Right Pointers in Each Node
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

1.  串联二叉树

# Populating Next Right Pointers in Each Node

​	[116. Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)



## Desc

You are given a **perfect binary tree** where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

```
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

**Example 2:**

```
Input: root = []
Output: []
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 212 - 1]`.
- `-1000 <= Node.val <= 1000`

 

**Follow-up:**

- You may only use constant extra space.
- The recursive approach is fine. You may assume implicit stack space does not count as extra space for this problem.

## Solution

### 1. 层序遍历

#### 思路

-  层序遍历添加到队列
-  依次连接相临节点

#### 解法 

```java
class Solution {

    public Node connect(Node root) {
        if (root == null) {
            return null;
        }
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                Node current = queue.poll();
                if (current == null) {
                    continue;
                }
                if (i < size - 1) {
                    current.next = queue.peek();
                } else {
                    current.next = null;
                }

                if (current.left != null) {
                    queue.add(current.left);
                }
                if (current.right != null) {
                    queue.add(current.right);
                }
            }
        }

        return root;
    }

}
```

  

#### 分析

- 需要额外空间，队列
- 涉及队列存取，消耗时间。

- 上面算法是连接本层节点，还可以连接子节点

### 2. 层序遍历

#### 思路

-  不依赖队列，而是在树本身做操作
-  分层遍历，连接下层子节点

#### 解法 

```java
class Solution {

    public Node connect(Node root) {
        if (root == null) {
            return null;
        }

        // 每一层的最左侧节点
        Node rootLeft = root;
        // 当前层游标
        Node current = rootLeft;

        // 层序遍历
        while (current.left != null) {
            // 连接左右子节点
            current.left.next = current.right;
            // 连接右子节点与后继节点的左子节点
            if (current.next != null) {
                current.right.next = current.next.left;
            }

            // 本层还有节点，后移
            if (current.next != null) {
                current = current.next;
            } else {
                // 移到下一层
                rootLeft = rootLeft.left;
                current = rootLeft;
            }
        }
        return root;
    }

}
```

   

#### 分析

- 不需要额外空间
- 效率最高

 

### 3. 递归

#### 思路

-  递归连接两节点
-  两个节点可以分别是同父左右子节点，异父相邻节点

#### 解法 

```java
class Solution {

    // 主函数
    public Node connect(Node root) {
        if (root == null) return null;
        connectTwoNode(root.left, root.right);
        return root;
    }

    // 辅助函数
    private void connectTwoNode(Node node1, Node node2) {
        if (node1 == null || node2 == null) {
            return;
        }
        // 将传入的两个节点连接
        node1.next = node2;

        // 连接相同父节点的两个子节点
        connectTwoNode(node1.left, node1.right);
        connectTwoNode(node2.left, node2.right);
        // 连接跨越父节点的两个子节点
        connectTwoNode(node1.right, node2.left);
    }

}
```

   

#### 分析

- 由于递归，需要栈作为额外空间，但题目中提到可以忽视递归带来的空间消耗。



## Summary

- **写递归算法的关键是要明确函数的「定义」是什么，然后相信这个定义，利用这个定义推导最终结果，绝不要跳入递归的细节**。
- 根据某节点的操作推到递归或迭代的操作。
- 根据某层的操作推导出迭代的操作。



## Reference

[手把手刷二叉树第一期](https://labuladong.github.io/algo/2/18/22/)

