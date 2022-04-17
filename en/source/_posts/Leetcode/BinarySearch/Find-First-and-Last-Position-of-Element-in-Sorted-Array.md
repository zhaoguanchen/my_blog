---
title: Find First and Last Position of Element in Sorted Array
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

# Find First and Last Position of Element in Sorted Array

Num: 34. Find First and Last Position of Element in Sorted Array

Link: https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/



## Desc

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

 

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

**Example 3:**

```
Input: nums = [], target = 0
Output: [-1,-1]
```

 

## Solution

### 1. Binary Search

##### 解法 

```java
class Solution {
  
  
    public int[] searchRange(int[] nums, int target) {
        // 分别考虑左边界和又边界
        int leftBound = searchLeftBound(nums, target);
        int rightBound = searchRightBound(nums, target);

        return new int[]{leftBound, rightBound};
    }

    public int searchLeftBound(int[] nums, int target) {
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
            // 向左移动右边界 试图找到第一个小于target的数值
            if (midValue == target) {
                right = mid - 1;
            } else if (midValue < target) {
                // 只取右边子数组，防止无限循环
                left = mid + 1;
            } else {
                // 只取左边子数组，防止无限循环
                right = mid - 1;
            }
        }
        // 检查越界
        if (right + 1 >= nums.length || nums[right + 1] != target) {
            return -1;
        }
        return right + 1;
    }

    public int searchRightBound(int[] nums, int target) {
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
            // 向右移动左边界 试图找到第一个大于target的数值
            if (midValue == target) {
                left = mid + 1;
            } else if (midValue < target) {
                // 只取右边子数组，防止无限循环
                left = mid + 1;
            } else {
                // 只取左边子数组，防止无限循环
                right = mid - 1;
            }
        }

        // 检查越界
        if (left - 1 < 0 || nums[left - 1] != target) {
            return -1;
        }
        return left - 1;
    }
}
```

   

## Summary

- 对于二分查找，算法不难但重在边界问题
- 循环结束条件`left <= right`保证子数组仅有一个元素时循环不会跳出，漏掉结果。
- `left = mid + 1`仅搜索左侧区间，避免死循环。比如，当区间为`[0,1]`时，`mid`为1。若`nums[1] > target`, 则`right`应设置为1, 此时出现死循环。
- `int mid = left + (right - left) / 2`可以防止`left + right`溢出。
- 找到元素值等于target时，收缩区间。





## Reference

[二分搜索](https://labuladong.github.io/algo/1/10/)

