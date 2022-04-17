---
title: Palindrome Linked List
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

判断链表是不是回文串。

# Palindrome Linked List

Num:  234. Palindrome Linked List

Link: https://leetcode.com/problems/palindrome-linked-list/



## Desc

Given the `head` of a singly linked list, return `true` if it is a palindrome.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```
Input: head = [1,2,2,1]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

```
Input: head = [1,2]
Output: false
```

 

**Constraints:**

- The number of nodes in the list is in the range `[1, 105]`.
- `0 <= Node.val <= 9`

 

**Follow up:** Could you do it in `O(n)` time and `O(1)` space?

## Solution

### 1. 递归全串比较

#### 思路

-  递归到最底，与头节点比较。递归回溯时，与头节点next比较。

#### 解法 

```java
class Solution {

		// 正序节点指针
    private static ListNode first;

    /**
     * 全比较
     * 递归到最底，与头节点比较。递归回溯时，与头节点next比较。
     * 这样实现了从后往前与从前往后的比较。
     *
     * @param head
     * @return
     */
    public static boolean isPalindrome(ListNode head) {
        first = head;
        return helper(head);
    }

    public static boolean helper(ListNode head) {
        // 到达末尾，无需比较，返回true
        if (head == null) {
            return true;
        }

        // 先递归，到达尾节点
        boolean res = helper(head.next);
        // 尾节点与头节点比较
        res = head.val == first.val && res;
        // 准备回溯，头节点后移，准备比较倒数第二节点与第二节点
        first = first.next;
        return res;
    }
  
}
```

   

#### 分析

- 空间复杂度为O(n), 这是因为递归调用了n次。



### 2. 翻转后半段

#### 思路

-  快慢指针法找到中间节点，翻转后半段（迭代法翻转链表）
-  原链表相当于分成了两段，从前往后比较即可。

#### 解法 

```java
    /**
     * 翻转后半段的解法
     *
     * @param head
     * @return
     */
    public static boolean isPalindrome(ListNode head) {
        ListNode mid = getMid(head);
        ListNode secondList = reverse(mid);
        ListNode p1 = head;
        ListNode p2 = secondList;
        while (p1 != null && p2 != null) {
            if (p1.val != p2.val) {
                return false;
            }
            p1 = p1.next;
            p2 = p2.next;
        }
        return true;
    }

    /**
     * 获取中间节点
     *
     * @param head
     * @return
     */
    private static ListNode getMid(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }


    /**
     * 翻转链表   迭代
     *
     * @param head
     * @return
     */
    private static ListNode reverse(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode first = head;
        ListNode second = head.next;
        // 虚拟头节点
        ListNode vHead = new ListNode(0);
        vHead.next = first;

        while (second != null) {
            first.next = second.next;
            // 新节点指向头部
            second.next = vHead.next;
            // 新节点链接到虚拟头节点后面
            vHead.next = second;
            second = first.next;
        }
        return vHead.next;
    }
```





### 其他

- 还可以将链表转化为数组，再通过双指针判断是否回文。
- 翻转链表还可以写为递归写法，但空间复杂度不是O(1)。

## Summary

- 简单解法可以直接转换为数组，通过双指针判断。
- 要优化空间复杂度，则需要在链表本身做操作。



## Reference

[如何判断回文链表](https://labuladong.github.io/algo/2/17/19/)

