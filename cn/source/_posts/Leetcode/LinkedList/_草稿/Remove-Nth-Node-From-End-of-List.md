---
title:  Remove Nth Node From End of List
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

# Remove Nth Node From End of List

Num:19. Remove Nth Node From End of List

Link: https://leetcode.com/problems/remove-nth-node-from-end-of-list/



## Desc

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

**Example 2:**

```
Input: head = [1], n = 1
Output: []
```

**Example 3:**

```
Input: head = [1,2], n = 1
Output: [1]
```

 

**Constraints:**

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

 

## Solution

### 1. 快慢指针

##### 思路

- 建立快慢指针指向头节点。
- fast指针先走n步，然后slow指针与fast指针同步前进，当fast到达链表尾部时，slow指针到达倒数第n个节点。
- 略微调整，使slow指向倒数第n+1节点



##### 解法 

```java
class Solution {

    public ListNode removeNthFromEnd(ListNode head, int n) {
        // 因为n可能等于链表长度（可能删除头节点），需要虚拟头节点
        ListNode vHead = new ListNode(-1);
        vHead.next = head;

        ListNode fast = vHead;
        ListNode slow = vHead;
        // 快指针先走n+1步（多走一步是为了让慢指针指向待删除节点的前一节点）
        for (int i = 0; i <= n; i++) {
            fast = fast.next;
        }
        // 慢指针与快指针同步前进，当快指针到达链表结尾时，
        // 慢指针到达倒数第n+1个节点，为待删除节点的前一节点
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }

        // 删除对应节点
        slow.next = slow.next.next;
        return vHead.next;
    }

  
}
```

   

 

   

## Summary

- 快慢指针为`one pass`,只需要遍历一遍
- 同时也可以考虑`two pass`，先获取链表长度 L，再找到第 L - n 个节点。





## Reference

[一文搞懂单链表的六大解题套路](https://labuladong.github.io/algo/2/17/16/)

