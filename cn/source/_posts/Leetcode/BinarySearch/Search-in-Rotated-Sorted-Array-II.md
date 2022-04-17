---
title: Search in Rotated Sorted Array II
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

# Search in Rotated Sorted Array II

Num: 81.  Search in Rotated Sorted Array II

Link: https://leetcode.com/problems/search-in-rotated-sorted-array-ii/



## Desc

There is an integer array `nums` sorted in non-decreasing order (not necessarily with **distinct** values).

Before being passed to your function, `nums` is **rotated** at an unknown pivot index `k` (`0 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,4,4,5,6,6,7]` might be rotated at pivot index `5` and become `[4,5,6,6,7,0,1,2,4,4]`.

Given the array `nums` **after** the rotation and an integer `target`, return `true` *if* `target` *is in* `nums`*, or* `false` *if it is not in* `nums`*.*

You must decrease the overall operation steps as much as possible.

 

**Example 1:**

```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```

**Example 2:**

```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```



**与1相比，增加了重复值，会导致`1,1,0,1,1,1,1,1`情况发生。**



## Solution

### 1. Binary Search

##### 思路

- 找到有序子区间，根据区间范围判断target是否在该区间内

- 无法确定有序子区间时，排除边界元素。

##### 解法 

```java
class Solution {
  
  
    public boolean search(int[] nums, int target) {
        if (nums.length == 0) {
            return false;
        }
        int left = 0;
        int right = nums.length - 1;
        while (left <= right) {
            while (left < right && nums[left] == nums[left + 1])
                ++left;
            while (left < right && nums[right] == nums[right - 1])
                --right;
            int mid = left + ((right - left) >> 1);
            int midValue = nums[mid];

            if (midValue == target) {
                return true;
            } else if (nums[mid] > nums[left]) {// 左区间有序（mid 在有序区间）
                // 如果target在左侧区间
                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            } else if (nums[mid] < nums[left]) { // 右侧区间一定有序
                // 如果target在右侧区间
                if (target <= nums[right] && target > nums[mid]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            } else { // 中间值与边界值相等，无法判断哪个子区间有序
                // 左区间收缩，逐步去掉重复值
                left++;
            }
        }

        return false;
    }
  
}
```

   

## Summary

- 对于二分查找，算法不难但重在边界问题
- 循环结束条件`left <= right`保证子数组仅有一个元素时循环不会跳出，漏掉结果。
- `left = mid + 1`仅搜索左侧区间，避免死循环。比如，当区间为`[0,1]`时，`mid`为1。若`nums[1] > target`, 则`right`应设置为1, 此时出现死循环。
- `int mid = left + (right - left) / 2`可以防止`left + right`溢出。
- 利用移位操作`>>`代替除法`/`，有一定效率提升。





## Reference

[二分搜索](https://labuladong.github.io/algo/1/10/)

