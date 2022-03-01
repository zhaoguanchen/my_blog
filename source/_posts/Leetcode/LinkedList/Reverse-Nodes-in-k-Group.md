---
title: Reverse Nodes in k-Group
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

k个一组翻转链表。

# Reverse Nodes in k-Group

Num:  25. Reverse Nodes in k-Group

Link: https://leetcode.com/problems/reverse-linked-list/



## Desc

- Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return *the modified list*.

  `k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

  You may not alter the values in the list's nodes, only nodes themselves may be changed.

   

  **Example 1:**

  ![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

  ```
  Input: head = [1,2,3,4,5], k = 2
  Output: [2,1,4,3,5]
  ```

  **Example 2:**

  ![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

  ```
  Input: head = [1,2,3,4,5], k = 3
  Output: [3,2,1,4,5]
  ```

   

  **Constraints:**

  - The number of nodes in the list is `n`.
  - `1 <= k <= n <= 5000`
  - `0 <= Node.val <= 1000`

   

  **Follow-up:** Can you solve the problem in `O(1)` extra memory space?

## Solution

### 思路

-  k个一组，先翻转子区间，再递归处理后续区间，直到区间子节点数量不足k。
-  翻转链表可以采用递归或者迭代，与之前翻转链表中实现类似。
-  base case: 区间子节点数量不足k，直接返回即可。

### 解法 

```java
class Solution {

    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null) {
            return head;
        }

        ListNode a = head;
        ListNode b = head;

        for (int i = 0; i < k; i++) {
            // 不足k个，直接返回
            if (b == null) {
                return head;
            }
            b = b.next;

        }
        // 翻转当前区间
        ListNode newHead = reverseBetween(a, b);
        // 此时a为当前区间的尾节点，后续接下一区间头节点
        a.next = reverseKGroup(b, k);
        return newHead;

    }

    /**
     * 翻转链表的迭代写法
     * 交换a节点到b节点，不包含b
     * <p>
     * 切记不是依次翻转。每个新节点都放在开头，而不是跟前一个节点交换位置
     *
     * @param a
     * @param b
     * @return
     */
    private ListNode reverseBetween(ListNode a, ListNode b) {
        ListNode vHead = new ListNode(-1);
        vHead.next = a;
        ListNode second = a.next;
        while (second != b) {
            a.next = second.next;
            second.next = vHead.next;
            vHead.next = second;
            second = a.next;
        }
        return vHead.next;
    }

  
}
```

   

### 其他

翻转链表还可以写为递归写法，更简单，不容易出错。

```java
    /**
     * 翻转链表的递归写法
     *
     * @param a
     * @param b
     * @return
     */
    private ListNode reverseBetween(ListNode a, ListNode b) {
        // 只剩下最后一个节点自己，直接返回
        if (a.next == b) {
            return a;
        }

        ListNode last = reverseBetween1(a.next, b);
        // 将a链接到末尾
        a.next.next = a;
        a.next = b;
        return last;
    }
```





## Summary

- 分组翻转
- 翻转建议采用递归写法，避免出错
- 翻转算法还需要多写几次，避免出错。
- 切记不是依次翻转。每个新节点都放在开头，而不是跟前一个节点交换位置

## Reference

[如何k个一组翻转链表](https://labuladong.github.io/algo/2/17/18/)

