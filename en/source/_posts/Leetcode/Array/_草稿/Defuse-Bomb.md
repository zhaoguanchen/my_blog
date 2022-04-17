---
title: Defuse the Bomb
date: 
updated:
tags: Array, sliding window
categories: Leetcode/Array
keywords: Array, sliding window
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

# Defuse the Bomb

Num: 1652. Defuse the Bomb

Link: https://leetcode.com/problems/defuse-the-bomb/



## Desc

You have a bomb to defuse, and your time is running out! Your informer will provide you with a **circular** array `code` of length of `n` and a key `k`.

To decrypt the code, you must replace every number. All the numbers are replaced **simultaneously**.

- If `k > 0`, replace the `ith` number with the sum of the **next** `k` numbers.
- If `k < 0`, replace the `ith` number with the sum of the **previous** `k` numbers.
- If `k == 0`, replace the `ith` number with `0`.

As `code` is circular, the next element of `code[n-1]` is `code[0]`, and the previous element of `code[0]` is `code[n-1]`.

Given the **circular** array `code` and an integer key `k`, return *the decrypted code to defuse the bomb*!

 

**Example 1:**

```
Input: code = [5,7,1,4], k = 3
Output: [12,10,16,13]
Explanation: Each number is replaced by the sum of the next 3 numbers. The decrypted code is [7+1+4, 1+4+5, 4+5+7, 5+7+1]. Notice that the numbers wrap around.
```

**Example 2:**

```
Input: code = [1,2,3,4], k = 0
Output: [0,0,0,0]
Explanation: When k is zero, the numbers are replaced by 0. 
```

**Example 3:**

```
Input: code = [2,4,9,3], k = -2
Output: [12,5,6,13]
Explanation: The decrypted code is [3+9, 2+3, 4+2, 9+4]. Notice that the numbers wrap around again. If k is negative, the sum is of the previous numbers.
```

  

## Solution

### 1. Sliding Window

##### 思路

- 指定滑动窗口，像右侧移动
- k是否大于0只会影响滑动窗口的起始位置



##### 解法 

```java
class Solution {
  
    public static int[] decrypt(int[] code, int k) {
        int l = code.length;
        // k==0时输入0。  数组默认值为0，直接返回空数组即可
        if (k == 0) {
            return new int[l];
        }
        int[] result = new int[l];
        // 作为sum的字串起点和终点
        int start;
        int end;
        int sum = 0;

        // k小于0时，取前k个值的和。则当i=0时，sum为数组倒数k个值的和
        if (k < 0) {
            k = -k;
            start = l - k;
            end = l - 1;
        } else {
            start = 1;
            end = k;
        }
        // 计算当i=0时，sum数组的值。同时作为缓存，避免重复计算。
        for (int i = start; i <= end; i++) {
            int index = i % l;
            sum += code[index];
        }

        // 随着i增大，sum子数组随着向后移位。
        for (int i = 0; i < l; i++) {
            result[i] = sum;
            // 起点先减掉再后移
            sum -= code[start];
            start = (start + 1) % l;
            // 终点先后移再增加
            end = (end + 1) % l;
            sum += code[end];
        }

        return result;
    }

}
```

   

## Summary

无



## Reference

无