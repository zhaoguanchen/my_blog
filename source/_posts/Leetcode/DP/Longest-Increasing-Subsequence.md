---
title: Longest Increasing Subsequence
date: 
updated:
tags: DP
categories: 
  - Leetcode
  - DP
keywords: DP Subsequence
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

# Longest Increasing Subsequence

Num: 300. Longest Increasing Subsequence

Link: https://leetcode.com/problems/longest-increasing-subsequence/



## Desc

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

 

**Example 1:**

```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

**Example 2:**

```
Input: nums = [0,1,0,3,2,3]
Output: 4
```

**Example 3:**

```
Input: nums = [7,7,7,7,7,7,7]
Output: 1
```

   

## Solution

### 1. 迭代法DP

##### 思路

- 某节点的最长子序列，为他前面的最大子序列（如最大子序列节点值小于该节点，则包含本身，加一）
- 自底向上进行迭代



##### 解法 

```java
class Solution {
  
  
    public static int lengthOfLIS(int[] nums) {
        int[] count = new int[nums.length];
        // 第一位子序列为1
        count[0] = 1;

        // 依次往后求最长子序列  当前边某节点小于当前节点时，可认为是子序列，求出最大的解
        for (int i = 1; i < nums.length; i++) {
            int max = 0;
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    max = Math.max(count[j], max);
                }
            }
            count[i] = max + 1;
        }

        // 找出结果数组中最大值即为最长子序列
        int res = 0;
        for (int j : count) {
            res = Math.max(res, j);
        }
        return res;

    }
  
}
```

   

## Summary

- 小问题推出大问题，找到递推关系
- 利用数组，map存储已计算节点





## Reference

[动态规划设计：最长递增子序列](https://labuladong.github.io/algo/3/23/73/)

