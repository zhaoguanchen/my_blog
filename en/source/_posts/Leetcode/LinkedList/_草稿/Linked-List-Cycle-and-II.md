---
title:  Linked List Cycle and II
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

# Linked List Cycle

Num:141. Linked List Cycle

Link: https://leetcode.com/problems/linked-list-cycle/



## Desc

- Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

  There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

  Return `true` *if there is a cycle in the linked list*. Otherwise, return `false`.

   

  **Example 1:**

  ![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

  ```
  Input: head = [3,2,0,-4], pos = 1
  Output: true
  Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
  ```

  **Example 2:**

  ![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

  ```
  Input: head = [1,2], pos = 0
  Output: true
  Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
  ```

  **Example 3:**

  ![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

  ```
  Input: head = [1], pos = -1
  Output: false
  Explanation: There is no cycle in the linked list.
  ```

   

  **Constraints:**

  - The number of the nodes in the list is in the range `[0, 104]`.
  - `-105 <= Node.val <= 105`
  - `pos` is `-1` or a **valid index** in the linked-list.



## Solution

### 1. 缓存迭代

##### 思路

- 采用set存储已走过的节点
- 当新节点等于已走过节点时，该链表有环



##### 解法 

```java
class Solution {

    public boolean hasCycle(ListNode head) {
      	Set<ListNode> set = new HashSet<>();
        ListNode cur = head;

        while (cur != null) {
            if (set.contains(cur)) {
                return true;
            }
            set.add(cur);
            cur = cur.next;
        }

        return false;
    }
  
}
```

   

### 2. 快慢指针

##### 思路

- 快指针每次走两步，慢指针每次走一步
- 快指针遇到慢指针，该链表有环



##### 解法 

```java
class Solution {
  
  
    public boolean hasCycle(ListNode head) {
        if (head == null) {
            return false;
        }
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            // 指针相遇
            if (fast == slow) {
                break;
            }

        }
        // 非指针相遇，而是走到了尽头
        if (fast == null || fast.next == null) {
            return false;
        }

        return true; 

    }

 
  
}
```





# Linked List Cycle II

Num:142. Linked List Cycle II

Link:https://leetcode.com/problems/linked-list-cycle-ii/



## Desc

Given the `head` of a linked list, return *the node where the cycle begins. If there is no cycle, return* `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

 

**Constraints:**

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

 

**Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?



## Solution

### 1. 缓存迭代

##### 思路

- 采用set存储已走过的节点
- 当新节点等于已走过节点时，该链表有环，当前节点即为环的起点



##### 解法 

```java
class Solution {

    public static ListNode detectCycle1(ListNode head) {
        Set<ListNode> set = new HashSet<>();
        ListNode cur = head;

        while (cur != null) {
            // 发现第一个已经走过的节点，即为环开始的地方
            if (set.contains(cur)) {
                return cur;
            }
            set.add(cur);
            cur = cur.next;
        }

        return null;
    }  
}
```

   

### 2. 快慢指针

##### 思路

- 快指针每次走两步，慢指针每次走一步
- 快指针遇到慢指针，该链表有环
- 将其中一个指针指向头节点，两节点同步前进
- 再次相遇时即为环的起点



##### 解法 

```java
class Solution {
  
  
    public static ListNode detectCycle(ListNode head) {
        // 快慢指针初始化指向 head
        ListNode slow = head, fast = head;
        // 快指针走到末尾时停止
        while (fast != null && fast.next != null) {
            // 慢指针走一步，快指针走两步
            slow = slow.next;
            fast = fast.next.next;
            // 快慢指针相遇，说明含有环
            if (slow == fast) {
                break;
            }
        }
        if (fast == null || fast.next == null) {
            return null;
        }
        slow = head;
        while (slow != fast) {
            fast = fast.next;
            slow = slow.next;
        }

        return fast;

    }
 
  
}
```



   

## Summary

- 双指针法空间复杂度为O(1)





## Reference

[一文搞懂单链表的六大解题套路](https://labuladong.github.io/algo/2/17/16/)

