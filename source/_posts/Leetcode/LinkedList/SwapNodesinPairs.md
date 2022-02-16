---
title: Swap Nodes in Pairs
date: 
updated:
tags: Linked List
categories: Leetcode
keywords: Linked List
description: 交换链表相邻节点
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

# 24. Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)





## 迭代

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
    public ListNode swapPairs(ListNode head) {
        // 判断空
        if (head == null || head.next == null) {
            return head;
        }
        // 新链表头节点
        ListNode root = new ListNode(-1);
        root.next = head;
        ListNode base = root;

        while (head != null && head.next != null) {
            ListNode backNode = head;
            ListNode frontNode = head.next;
            // 交换节点
            base.next = frontNode;
            backNode.next = frontNode.next;
            frontNode.next = backNode;

            // 基准节点后移，head节点后移
            base = frontNode.next;
            head = base.next;
        }

        return root.next;

    }
}
```



**Complexity Analysis**

- Time Complexity : *O(N)* , where N is the size of the linked list.
- Space Complexity : O(1).



## 递归

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode swapPairs(ListNode head) {
        // If the list has no node or has only one node left.
        if ((head == null) || (head.next == null)) {
            return head;
        }

        // Nodes to be swapped
        ListNode firstNode = head;
        ListNode secondNode = head.next;

        // Swapping
        firstNode.next  = swapPairs(secondNode.next);
        secondNode.next = firstNode;

        // Now the head is the second node
        return secondNode;
    }
}
```



**Complexity Analysis**

- Time Complexity : *O(N)* , where N is the size of the linked list.
- Space Complexity : *O(N)*. 

