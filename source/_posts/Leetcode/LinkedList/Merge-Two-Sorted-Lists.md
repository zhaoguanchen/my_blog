---
title: Merge Two Sorted Lists
date: 
updated:
tags: Linked List
categories: 
  - Leetcode
  - Linked List
keywords: Sorted List
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

# Merge Two Sorted Lists

Num: 21. Merge Two Sorted Lists

Link: https://leetcode.com/problems/merge-two-sorted-lists/



## Desc

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists in a one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

**Example 2:**

```
Input: list1 = [], list2 = []
Output: []
```

**Example 3:**

```
Input: list1 = [], list2 = [0]
Output: [0]
```

 

**Constraints:**

- The number of nodes in both lists is in the range `[0, 50]`.
- `-100 <= Node.val <= 100`
- Both `list1` and `list2` are sorted in **non-decreasing** order.

   



## Solution

### 1. 迭代法DP

##### 思路

- 建立虚拟节点
- 拉链法依次向右合并



##### 解法 

```java
class Solution {
  
  
    public static ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if (list1 == null && list2 == null) {
            return null;
        }
        // list指针
        ListNode p1 = list1;
        ListNode p2 = list2;
        // 虚拟头节点 指向结果链表
        ListNode tempHead = new ListNode();
        // 游标
        ListNode cur = tempHead;
        while (p1 != null && p2 != null) {
            if (p1.val < p2.val) {
                cur.next = p1;
                p1 = p1.next;
            } else {
                cur.next = p2;
                p2 = p2.next;
            }

            cur = cur.next;
        }
        // p1或p2到达末尾  可直接链接另一个list
        if (p1 == null) {
            cur.next = p2;
        } else {
            cur.next = p1;
        }
        // 去掉虚拟头节点
        return tempHead.next;
    }
  
}
```

   

## Summary

- 虚拟头节点
- 拉链法





## Reference

[一文搞懂单链表的六大解题套路](https://labuladong.github.io/algo/2/17/16/)

