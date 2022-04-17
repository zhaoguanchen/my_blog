---
title: Sqrt(x)
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

# Sqrt(x)

Num: 69. Sqrt(x)

Link: https://leetcode.com/problems/sqrtx/



## Desc

Given a non-negative integer `x`, compute and return *the square root of* `x`.

Since the return type is an integer, the decimal digits are **truncated**, and only **the integer part** of the result is returned.

**Note:** You are not allowed to use any built-in exponent function or operator, such as `pow(x, 0.5)` or `x ** 0.5`.

 

**Example 1:**

```
Input: x = 4
Output: 2
```

**Example 2:**

```
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.
```



## Solution

### 1. Binary Search

##### 思路

- 二分法查找目标元素
- 返回平方小于target的最大值



##### 解法 

```java
class Solution {
  
  
    public int searchInsert(int[] nums, int target) {
      // 个例处理
       if (x <= 1) {
            return x;
        }
        int left = 0;
      	// 移位代替/2操作
        int right = x >> 1;
        while (left <= right) {
            // 移位代替/2操作
            int mid = left + ((right - left) >> 1);
            double powValue = Math.pow(mid, 2);
            if (powValue == x) {
                return mid;
            } else if (powValue < x) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
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
- 利用移位操作`>>`代替除法`/`，有一定效率提升。





## Reference

[二分搜索](https://labuladong.github.io/algo/1/10/)

