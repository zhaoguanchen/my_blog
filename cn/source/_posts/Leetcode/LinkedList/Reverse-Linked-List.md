---
title: Reverse Linked List and II
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

本文涉及反转链表，反转链表前n个节点以及反转链表部分节点。

# 1. Reverse Linked List

Num:  206. Reverse Linked List

Link: https://leetcode.com/problems/reverse-linked-list/



## Desc

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
Input: head = [1,2]
Output: [2,1]
```

**Example 3:**

```
Input: head = []
Output: []
```

 

**Constraints:**

- The number of nodes in the list is the range `[0, 5000]`.

  

## Solution

### 1. 递归

##### 思路

-  递归处理，从后往前改变指针指向。
- 头节点指向null
-  base case: 如果链表只有一个节点的时候反转也是它自己，直接返回即可。



##### 解法 

```java
class Solution {

    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }

        // 递归反转，最终返回末尾元素
        ListNode last = reverseList(head.next);

        // head原来指向第二个节点，现在第二个节点指向head
        head.next.next = head;
        // 作为最后节点，head指向null
        head.next = null;
        return last;

    }

  
}
```

   

### 2. 迭代



##### 思路

-  从前往后，建立节点引用，改变指针指向
- 头节点指向null



##### 解法 

```java
class Solution {

    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        // 前一节点
        ListNode pre = null;
        // 游标节点
        ListNode cur = head;

        // 找到当前节点与下一节点，改变指向关系
        while (cur != null) {
            ListNode nextNode = cur.next;
            cur.next = pre;
            pre = cur;
            cur = nextNode;
        }

        // cur == null, pre为最后一个节点，作为头节点返回
        return pre;
    }
  
}
```

   

# 2. Reverse Linked List II

Num:  92. Reverse Linked List II

Link: https://leetcode.com/problems/reverse-linked-list-ii/



## Desc



- Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return *the reversed list*.

   

  **Example 1:**

  ![img](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

  ```
  Input: head = [1,2,3,4,5], left = 2, right = 4
  Output: [1,4,3,2,5]
  ```

  **Example 2:**

  ```
  Input: head = [5], left = 1, right = 1
  Output: [5]
  ```

   

  **Constraints:**

  - The number of nodes in the list is `n`.
  - `1 <= n <= 500`
  - `-500 <= Node.val <= 500`
  - `1 <= left <= right <= n`

   

  **Follow up:** Could you do it in one pass?

## Solution

### 1. 递归

##### 思路

-  将问题转化为反转前n个节点



##### 解法 

```java
class Solution {

    /**
     * 反转后链表的下一个节点
     */
    private ListNode nextNode;

    public ListNode reverseBetween(ListNode head, int left, int right) {
        if (head == null) {
            return head;
        }

        // 到达起点，题目转换为反转链表前n个节点
        if (left == 1) {
            return reverseTopN(head, right);
        }
        // 前进到要反转的区间
        head.next = reverseBetween(head.next, left - 1, right - 1);

        return head;
    }


    /**
     * reverse top n
     *
     * @param head
     * @param n
     * @return
     */
    private ListNode reverseTopN(ListNode head, int n) {
        if (n == 1) {
            nextNode = head.next;
            return head;
        }

        ListNode last = reverseTopN(head.next, n - 1);

        head.next.next = head;
        head.next = nextNode;

        return last;
    }
}
```

   

### 2. 迭代



##### 思路

-  从前往后，建立节点引用，改变指针指向
-  second节点不是指向前一节点（first），而是指向区间的前一节点。相当于该节点与前面的字串进行反转。



##### 解法 

```java
class Solution {


    public static ListNode reverseBetween(ListNode head, int left, int right) {
        if (head == null) {
            return head;
        }

        ListNode vHead = new ListNode(-1); // create a dummy node to mark the head of this list
        vHead.next = head;
        ListNode pre = vHead;

        // 前进到区间前一节点
        for (int i = 1; i < left; i++) {
            pre = pre.next;
        }

        ListNode first = pre.next;
        ListNode second = pre.next.next;

        int bar = right - left;
        for (int i = 0; i < bar; i++) {
            // 与字串反转
            first.next = second.next;
            second.next = pre.next;
            pre.next = second;
            second = first.next;
            ListNode.print(vHead);
        }


        return vHead.next;
    }



}
```

   

   

## Summary

- 建立对应节点引用，操作节点指向。
- 不要跳进递归（你的脑袋能压几个栈呀？），而是要根据函数定义，来弄清楚这段代码会产生什么结果：

## Reference

[一文搞懂单链表的六大解题套路](https://labuladong.github.io/algo/2/17/16/)

