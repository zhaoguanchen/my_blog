---
title: 二叉树的序列化和反序列化
date: 
updated:
tags: 
  - 二叉树
categories: 
  - Leetcode
  - 二叉树
keywords: 
  - 序列化 
  - 二叉树
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

# 二叉树的序列化和反序列化

 

我们可以用 `serialize` 方法将二叉树序列化成字符串，用 `deserialize` 方法将序列化的字符串反序列化成二叉树，至于以什么格式序列化和反序列化，都可以，只要二者能够配合使用进行编码和解码。





## 典型题解

### Serialize and Deserialize Binary Tree

#### 描述

这道题是Leetcode中[297. Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)，需要实现`serialize`和`deserialize`两个方法，对一个二叉树进行序列化和反序列化。

题目描述如下：

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Clarification:** The input/output format is the same as [how LeetCode serializes a binary tree](https://leetcode.com/faq/#binary-tree). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

 

**Example 1:**

 ![img](https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg)

```
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
```

**Example 2:**

```
Input: root = []
Output: []
```

 

#### 解法

**分隔符与Null**

```java
private final String splitStr = ",";
private final String nullStr = "#";
```

因为涉及到字符串拼接，我们需要指定数字之间的分隔符，以及用一个字符来表示`null`.

**StringBuilder**

`StringBuilder` 可以用于高效拼接字符串。利用`append()`追加字符串，利用`toString()`得到最终的字符串。



##### 1. 前序遍历

![Image](https://mmbiz.qpic.cn/sz_mmbiz_jpg/gibkIz0MVqdFJlkkg2icueWtNAuPtHuQ6vmOfpGWptTgHonJR8qH2TdSltf8jQ5mNMkQxm7gxicib8a9HBIibEibicL2Q/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)



通过二叉树的前序遍历，我们可以很容易看出第一个为root节点的值。因此，通过前序遍历得到的序列，我们可以还原出对应的二叉树。

```java

/**
 * 前序遍历
 */
class Solution {

    private final String splitStr = ",";
    private final String nullStr = "#";

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        serializeHelper(root, sb);
        return sb.toString();
    }

    private void serializeHelper(TreeNode root, StringBuilder sb) {
        if (root == null) {
            sb.append(nullStr);
            sb.append(splitStr);
            return;
        }
        sb.append(root.val);
        sb.append(splitStr);

        serializeHelper(root.left, sb);
        serializeHelper(root.right, sb);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] strings = data.split(splitStr);
        LinkedList<String> list = new LinkedList<>(Arrays.asList(strings));
        return deserializeHelper(list);
    }

    private TreeNode deserializeHelper(LinkedList<String> list) {
        if (list.isEmpty()) {
            return null;
        }
      	// 从前往后
        String s = list.pollFirst();
        if (s.equals(nullStr)) {
            return null;
        }
        int value = Integer.parseInt(s);
        TreeNode root = new TreeNode(value);
        root.left = deserializeHelper(list);
        root.right = deserializeHelper(list);

        return root;
    }

}
```



##### 2. 后序遍历

与前序遍历类似，后序遍历中，root节点对应的值再最后一个，因此从后往前依次递归，可得到完整的二叉树。需要注意的是，我们需要先递归右子树，在递归左子树。

```java

/**
 * 后序遍历
 */
class Postorder {

    private final String splitStr = ",";
    private final String nullStr = "#";

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        serializeHelper(root, sb);
        return sb.toString();
    }

    private void serializeHelper(TreeNode root, StringBuilder sb) {
        if (root == null) {
            sb.append(nullStr);
            sb.append(splitStr);
            return;
        }
        serializeHelper(root.left, sb);
        serializeHelper(root.right, sb);

        sb.append(root.val);
        sb.append(splitStr);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] strings = data.split(splitStr);
        LinkedList<String> list = new LinkedList<>(Arrays.asList(strings));
        return deserializeHelper(list);
    }

    private TreeNode deserializeHelper(LinkedList<String> list) {
        if (list.isEmpty()) {
            return null;
        }
      	// 从后往前
        String s = list.pollLast();
        if (s.equals(nullStr)) {
            return null;
        }
        int value = Integer.parseInt(s);
        TreeNode root = new TreeNode(value);
      	// 先右子树，再左子树
        root.right = deserializeHelper(list);
        root.left = deserializeHelper(list);
        return root;
    }

}
```



##### 3. 层序遍历

层序遍历，即按层将各节点储存起来，无节点的情况存null。

与二叉树的层序遍历一样，我们借助队列实现层序遍历。

```java

/**
 * BFS版本  层序遍历
 */
class BFS {

    private final String splitStr = ",";
    private final String nullStr = "#";

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        if (null == root) {
            return "";
        }
        StringBuilder stringBuilder = new StringBuilder();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (null == node) {
                stringBuilder.append(nullStr);
                stringBuilder.append(splitStr);
                continue;
            }

            stringBuilder.append(node.val);
            stringBuilder.append(splitStr);

            queue.offer(node.left);
            queue.offer(node.right);
        }

        return stringBuilder.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data.isEmpty()) {
            return null;
        }
        String[] strings = data.split(splitStr);
        int rootVal = Integer.parseInt(strings[0]);
        TreeNode root = new TreeNode(rootVal);
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        for (int i = 1; i < strings.length; ) {
            TreeNode node = queue.poll();
            // 左子节点
            String leftStr = strings[i];
            if (leftStr.equals(nullStr)) {
                node.left = null;
            } else {
                int leftVal = Integer.parseInt(strings[i]);
                TreeNode leftNode = new TreeNode(leftVal);
                node.left = leftNode;
                queue.add(leftNode);
            }

            i++;
            // 右子节点
            String rightStr = strings[i];
            if (rightStr.equals(nullStr)) {
                node.right = null;
            } else {
                int rightVal = Integer.parseInt(strings[i]);
                TreeNode rightNode = new TreeNode(rightVal);
                node.right = rightNode;
                queue.add(rightNode);
            }

            i++;
        }
        return root;
    }
}
```

