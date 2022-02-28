---
title: Reverse Linked List
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

# Reverse Linked List

Num:  206. Reverse Linked List

Link: https://leetcode.com/problems/intersection-of-two-linked-lists/



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
        ListNode last = reverseList1(head.next);

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

   



   

## Summary

- 建立对应节点引用，操作节点指向。
- 不要跳进递归（你的脑袋能压几个栈呀？），而是要根据函数定义，来弄清楚这段代码会产生什么结果：

## Reference

[一文搞懂单链表的六大解题套路](https://labuladong.github.io/algo/2/17/16/)

