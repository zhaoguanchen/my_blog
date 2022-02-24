---
title: Merge k Sorted Lists
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

# Merge k Sorted Lists

Num: 23. Merge k Sorted Lists

Link: https://leetcode.com/problems/merge-k-sorted-lists/



## Desc

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

*Merge all the linked-lists into one sorted linked-list and return it.*

 

**Example 1:**

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

**Example 2:**

```
Input: lists = []
Output: []
```

**Example 3:**

```
Input: lists = [[]]
Output: []
```

 

**Constraints:**

- `k == lists.length`
- `0 <= k <= 104`
- `0 <= lists[i].length <= 500`
- `-104 <= lists[i][j] <= 104`
- `lists[i]` is sorted in **ascending order**.
- The sum of `lists[i].length` will not exceed `104`.

## Solution

### 1. 迭代法DP

##### 思路

- 采用优先队列，或许每次比较节点的最小值



##### 解法 

```java
class Solution {
  
  
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) {
            return null;
        }
        // 优先队列
        PriorityQueue<ListNode> queue = new PriorityQueue<>(lists.length, Comparator.comparingInt(value -> value.val));

        // 虚拟头节点
        ListNode vHead = new ListNode(0);
        // 游标
        ListNode cur = vHead;

        // 将头节点放入
        for (ListNode list : lists) {
            if (list != null) {
                queue.add(list);
            }
        }

        // 最小节点出队后，添加其下一节点
        while (!queue.isEmpty()) {
            ListNode node = queue.poll();
            cur.next = node;
            cur = cur.next;
            if (node.next != null) {
                queue.add(node.next);
            }

        }

        return vHead.next;
    }
  
}
```

   

## Summary

- 虚拟头节点
- 拉链法
- 优先队列 最小堆





## Reference

[一文搞懂单链表的六大解题套路](https://labuladong.github.io/algo/2/17/16/)

