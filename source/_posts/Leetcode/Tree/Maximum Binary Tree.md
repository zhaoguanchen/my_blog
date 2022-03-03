---
title: Construct from traversal(Preorder, Inorder, Postorder)
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

 

本篇包含二叉树构建相关的题目，包括：

1.  构建最大二叉树
2. 由前序、中序遍历构建二叉树
3. 由中序、后序遍历构建二叉树
4. 由前序、后序遍历构建二叉树



这类题的关键是找到根节点位置，根据遍历属性找到左右子节点的所属区间，进而进行递归。

# Maximum Binary Tree

​	[654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)



## Desc

- You are given an integer array `nums` with no duplicates. A **maximum binary tree** can be built recursively from `nums` using the following algorithm:

  1. Create a root node whose value is the maximum value in `nums`.
  2. Recursively build the left subtree on the **subarray prefix** to the **left** of the maximum value.
  3. Recursively build the right subtree on the **subarray suffix** to the **right** of the maximum value.

  Return *the **maximum binary tree** built from* `nums`.

   

  **Example 1:**

  ![img](https://assets.leetcode.com/uploads/2020/12/24/tree1.jpg)

  ```
  Input: nums = [3,2,1,6,0,5]
  Output: [6,3,5,null,2,0,null,null,1]
  Explanation: The recursive calls are as follow:
  - The largest value in [3,2,1,6,0,5] is 6. Left prefix is [3,2,1] and right suffix is [0,5].
      - The largest value in [3,2,1] is 3. Left prefix is [] and right suffix is [2,1].
          - Empty array, so no child.
          - The largest value in [2,1] is 2. Left prefix is [] and right suffix is [1].
              - Empty array, so no child.
              - Only one element, so child is a node with value 1.
      - The largest value in [0,5] is 5. Left prefix is [0] and right suffix is [].
          - Only one element, so child is a node with value 0.
          - Empty array, so no child.
  ```

  **Example 2:**

  ![img](https://assets.leetcode.com/uploads/2020/12/24/tree2.jpg)

  ```
  Input: nums = [3,2,1]
  Output: [3,null,2,null,1]
  ```

- 

## Solution

### 1. 递归

#### 思路

-  找到最大节点构建
-  递归在左区间构建左子树，右区间构建右子树。

#### 解法 

```java
class Solution {


    private static int[] numArray;

    public static TreeNode constructMaximumBinaryTree(int[] nums) {
        numArray = nums;
        return helper(0, numArray.length - 1);

    }

    private static TreeNode helper(int left, int right) {
        // 不存在子节点，返回null
        if (left > right) {
            return null;
        }

        int maxValue = Integer.MIN_VALUE;
        int indexOfMax = -1;

        // 去当前区间最大值
        for (int i = left; i <= right; i++) {
            if (numArray[i] > maxValue) {
                maxValue = numArray[i];
                indexOfMax = i;
            }
        }
        // 构建节点
        TreeNode root = new TreeNode(maxValue);
        // 递归构建左右子树
        root.left = helper(left, indexOfMax - 1);
        root.right = helper(indexOfMax + 1, right);

        return root;
    }
}
```



#### 分析

- 很容易看出是递归。

 





# Construct Binary Tree from Preorder and Inorder Traversal

​	[105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)



## Desc

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return *the binary tree*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```



## Solution

### 1. 递归

#### 思路

-  根据前序遍历找到根节点。
-  根据中序遍历中根节点的位置，其左侧即为左子树区间，右侧为右子树区间。找到左右子区间。
-  对左右子区间递归，作为左右子节点。



下图为左右区间的分布情况。

![img](https://labuladong.github.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%91%e7%b3%bb%e5%88%972/3.jpeg)



#### 解法 

```java
class Solution {
  
    private int[] preorderArray;
    private int[] inorderArray;

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        preorderArray = preorder;
        inorderArray = inorder;
				// 初始位置
        int preorderStart = 0;
        int preorderEnd = preorderArray.length - 1;
        int inorderStart = 0;
        int inorderEnd = inorderArray.length - 1;
        return helper(preorderStart, preorderEnd, inorderStart, inorderEnd);
    }

    private TreeNode helper(int preorderStart, int preorderEnd, int inorderStart, int inorderEnd) {
        if (preorderStart > preorderEnd || inorderStart > inorderEnd) {
            return null;
        }

        // root节点为前序遍历中第一个
        int rootVal = preorderArray[preorderStart];

        // 在中序遍历数组中找到root位置
        int rootIndexInOrder = -1;
        for (int i = inorderStart; i <= inorderEnd; i++) {
            if (inorderArray[i] == rootVal) {
                rootIndexInOrder = i;
                break;
            }
        }
        // 构建当前节点
        TreeNode root = new TreeNode(rootVal);
				// 根据中序遍历数组，得到左区间的大小
        int leftSize = rootIndexInOrder - inorderStart;

        // 递归构建左右子树
        root.left = helper(preorderStart + 1, preorderStart + leftSize, inorderStart, rootIndexInOrder - 1);
        root.right = helper(preorderStart + leftSize + 1, preorderEnd, rootIndexInOrder + 1, inorderEnd);

        return root;
    }
}
```



#### 分析

- 找到两个数组中对应的区间至关重要，可以用例子画一画。



# Construct Binary Tree from Inorder and Postorder Traversal

​	[106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)



## Desc

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return *the binary tree*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: inorder = [-1], postorder = [-1]
Output: [-1]
```

 

**Constraints:**

- `1 <= inorder.length <= 3000`
- `postorder.length == inorder.length`
- `-3000 <= inorder[i], postorder[i] <= 3000`
- `inorder` and `postorder` consist of **unique** values.
- Each value of `postorder` also appears in `inorder`.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.
- `postorder` is **guaranteed** to be the postorder traversal of the tree.

## Solution

### 1. 递归

#### 思路

-  根据后序遍历找到根节点，根节点为后序遍历末尾元素。
-  在中序遍历中寻找根节点的位置，其左侧即为左子树区间，右侧为右子树区间。
-  对左右子区间递归，作为左右子节点。



下图为左右区间的分布情况。

![img](https://labuladong.github.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%91%e7%b3%bb%e5%88%972/6.jpeg)



#### 解法 

```java
class Solution {

    private static int[] postorderArray;
    private static int[] inorderArray;

    public static TreeNode buildTree(int[] inorder, int[] postorder) {
        inorderArray = inorder;
        postorderArray = postorder;

        // inorder起止
        int iStart = 0;
        int iEnd = inorderArray.length - 1;
        // postorder起止
        int pStart = 0;
        int pEnd = postorderArray.length - 1;
        return helper(iStart, iEnd, pStart, pEnd);
    }

    private static TreeNode helper(int iStart, int iEnd, int pStart, int pEnd) {
        // 结束条件
        if (iStart > iEnd || pStart > pEnd) {
            return null;
        }

        // root节点为后序遍历中最后一个
        int rootVal = postorderArray[pEnd];

        // 在中序遍历数组中找到root位置
        int rootIndexInOrder = -1;
        for (int i = iStart; i <= iEnd; i++) {
            if (inorderArray[i] == rootVal) {
                rootIndexInOrder = i;
                break;
            }
        }
        // 构建节点
        TreeNode root = new TreeNode(rootVal);

        // 左子树节点数量为中序遍历中根节点左侧的元素数量
        int leftSize = rootIndexInOrder - iStart;


        // 递归构建左右子树
        root.left = helper(iStart, rootIndexInOrder - 1, pStart, pStart + leftSize - 1);
        root.right = helper(rootIndexInOrder + 1, iEnd, pStart + leftSize, pEnd - 1);

        return root;
    }
}
```



#### 分析

- 找到两个数组中对应的区间至关重要，可以用例子画一画。





# Construct Binary Tree from Preorder and Postorder Traversall

​	[889. Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)



## Desc

Given two integer arrays, `preorder` and `postorder` where `preorder` is the preorder traversal of a binary tree of **distinct** values and `postorder` is the postorder traversal of the same tree, reconstruct and return *the binary tree*.

If there exist multiple answers, you can **return any** of them.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/07/24/lc-prepost.jpg)

```
Input: preorder = [1,2,4,5,3,6,7], postorder = [4,5,2,6,7,3,1]
Output: [1,2,3,4,5,6,7]
```

**Example 2:**

```
Input: preorder = [1], postorder = [1]
Output: [1]
```



## Solution

### 1. 递归

#### 思路

-  根据前序遍历找到根节点。
-  前序遍历中根节点后面跟着的就是左子根节点。
-  在后序遍历中找到左子根节点的位置，其左边即为左子树，右边为右子树。



下图为左右区间的分布情况。

![img](https://labuladong.github.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%91%e7%b3%bb%e5%88%972/8.jpeg)



#### 解法 

```java
class Solution {

    private int[] preorderArray;
    private int[] postorderArray;

    public TreeNode constructFromPrePost(int[] preorder, int[] postorder) {
        preorderArray = preorder;
        postorderArray = postorder;

        // preorder起止
        int preStart = 0;
        int preEnd = preorder.length - 1;
        // postorder起止
        int postStart = 0;
        int postEnd = postorderArray.length - 1;
        return helper(preStart, preEnd, postStart, postEnd);
    }

    private TreeNode helper(int preStart, int preEnd, int postStart, int postEnd) {
        // 结束条件
        if (preStart > preEnd || postStart > postEnd) {
            return null;
        }

        // root节点为前序遍历的第一个；同时也是后序遍历最后一个。
        int rootVal = preorderArray[preStart];
        // 构建节点
        TreeNode root = new TreeNode(rootVal);

        if (preStart == preEnd) {
            return root;
        }

        // 左子root为前序遍历中下一节点
        int leftVal = preorderArray[preStart + 1];

        // 在后序遍历数组中找到leftVal位置
        int leftIndexPostorder = -1;
        for (int i = postStart; i <= postEnd; i++) {
            if (postorderArray[i] == leftVal) {
                leftIndexPostorder = i;
                break;
            }
        }

        // 左子树节点数量为后序遍历中左子根节点左侧的元素数量
        int leftSize = leftIndexPostorder - postStart + 1;

        // 递归构建左右子树
        root.left = helper(preStart + 1, preStart + leftSize, postStart, postStart + leftSize - 1);
        root.right = helper(preStart + leftSize + 1, preEnd, postStart + leftSize, postEnd - 1);

        return root;
    }

}
```



#### 分析

- 找到两个数组中对应的区间至关重要，可以用例子画一画。
- 答案不唯一

## 



## Summary

- 这类题的关键是找到根节点位置，根据遍历属性找到左右子节点的所属区间，进而进行递归。







## Reference

[手把手刷二叉树第二期](https://labuladong.github.io/algo/2/18/23/)

