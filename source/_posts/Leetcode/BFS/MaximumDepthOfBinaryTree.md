---
title: Maximum Depth of Binary Tree
date: 
updated:
tags: 
  - BFS
  - DFS
  - Binary Tree
categories: 
  - Leetcode
  - BFS
keywords: 
  - BFS
  - DFS
  - Binary Tree
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

# Maximum Depth of Binary Tree

Num: 104. Maximum Depth of Binary Tree

Link: https://leetcode.com/problems/maximum-depth-of-binary-tree/



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
- 自顶向下按层循环，记录当前层的深度。最后一层遍历完毕，即为最大深度。

##### 解法 

```java
class Solution {
  
   /**
     * BFS
     */
    public int maxDepth(TreeNode root) {
        int depth = 0;
        if (root == null) {
            return 0;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            depth++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode currentNode = queue.poll();
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
    private int maxDepth = 0;

    public int maxDepth(TreeNode root) {
        helper(root, 0);
        return maxDepth;
    }

    private void helper(TreeNode root, int depth) {
        // 走到叶子节点，记录最大深度
        if (root == null) {
            maxDepth = Math.max(maxDepth, depth);
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

### 3. 双队列迭代法BFS

##### 思路

- 采用双队列存储节点和对应节点的深度
- BFS层序遍历，本层遍历完毕，将子节点加入队列



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
  
   public static int minDepthIterator(TreeNode root) {
        if (root == null) {
            return 0;
        }

        Queue<TreeNode> nodeQueue = new LinkedList<>();
        Queue<Integer> depthQueue = new LinkedList<>();

        nodeQueue.add(root);
        depthQueue.add(1);

        int max = 0;

        while (!nodeQueue.isEmpty()) {
            TreeNode currentNode = nodeQueue.poll();
            Integer currentDepth = depthQueue.poll();
            if (currentDepth == null) {
                continue;
            }

            max = Math.max(currentDepth, max);

            if (currentNode.left != null) {
                nodeQueue.add(currentNode.left);
                depthQueue.add(currentDepth + 1);
            }

            if (currentNode.right != null) {
                nodeQueue.add(currentNode.right);
                depthQueue.add(currentDepth + 1);
            }
        }

        return max;
    }

}
```

 





## Reference

[BFS 算法解题套路框架] https://labuladong.gitee.io/algo/1/6/