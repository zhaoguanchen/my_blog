---
title: Swapping Nodes in a Linked List
date: 
updated:
tags: Linked List
categories: Leetcode
keywords: Linked List
description: 交换链表中指定节点
top_img:
comments: true
cover:
toc: true
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

# Swapping Nodes in a Linked List

1721.Swapping Nodes in a Linked List

Link: https://leetcode.com/problems/swapping-nodes-in-a-linked-list/

## Desc

You are given the `head` of a linked list, and an integer `k`.

Return *the head of the linked list after **swapping** the values of the* `kth` *node from the beginning and the* `kth` *node from the end (the list is **1-indexed**).*

**Example**

```ja
Input: head = [1,2,3,4,5], k = 2
Output: [1,4,3,2,5]
```

```java
Input: head = [7,9,6,6,7,8,3,0,9,5], k = 5
Output: [7,9,6,6,8,7,3,0,9,5]
```



## Solution

### 交换值

简单起见，可以仅考虑交换两节点的值，这种方法可以通过所有的test case。



可以采用3次遍历确定左侧节点和右侧节点，然后交换值，也可以2次遍历获得，甚至一次遍历得到。



**一趟遍历的解法**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode swapNodes(ListNode head, int k) {
        ListNode cur = head;
        int length = 0;
        // 左侧节点
        ListNode left = head;
        // 右侧节点
        ListNode right = head;
        // iteration
        while (cur.next != null) {
            length++;
            // 当左侧节点为第k个节点时，糨右侧节点设为head节点。这样，当遍历结束时，右侧节点恰好为length - k所在的节点。
            if (length == k) {
                left = cur;
                right = head;
            }

            right = right.next;
            cur = cur.next;
        }

        // exchange
        int temp = left.val;
        left.val = right.val;
        right.val = temp;

        return head;
    }
}
```



### 交换节点

Swap the actual nodes.

本题应该旨在考察交换节点，所以进一步尝试一下交换节点的解法。



```java



```

