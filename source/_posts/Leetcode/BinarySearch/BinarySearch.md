---
title: Binary Search
date: 
updated:
tags: Binary Search
categories: Leetcode/Binary Search
keywords: Binary Search
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

# Binary Search

Num: 704. Binary Search

Link: https://leetcode.com/problems/binary-search/



## Desc

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

 

**Example 1:**

```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
```

**Example 2:**

```
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
```

  

## Solution

### 1. Binary Search

##### 解法 

```java
class Solution {
  
    public int search(int[] nums, int target) {
        if (nums.length == 0) {
            return -1;
        }
        int left = 0;
        int right = nums.length - 1;
        // = 是为了防止数组中只有一个值时跳出的问题（此时left == right）
        while (left <= right) {
            // 为什么不是（left + right）/2
            // 防止相加和溢出
            int mid = left + (right - left) / 2;
            int midValue = nums[mid];
            if (midValue == target) {
                return mid;
            } else if (midValue < target) {
                // 只取右边子数组，防止无限循环
                left = mid + 1;
            } else {
                // 只取左边子数组，防止无限循环
                right = mid - 1;
            }
        }
        return -1;
    }

}
```

   

## Summary

- 对于二分查找，算法不难但重在边界问题
- 循环结束条件`left <= right`保证子数组仅有一个元素时循环不会跳出，漏掉结果。
- `left = mid + 1`仅搜索左侧区间，避免死循环。比如，当区间为`[0,1]`时，`mid`为1。若`nums[1] > target`, 则`right`应设置为1, 此时出现死循环。
- `int mid = left + (right - left) / 2`可以防止`left + right`溢出。





## Reference

[二分搜索][https://labuladong.github.io/algo/1/10/]