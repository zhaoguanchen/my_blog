---
title: Search a 2D Matrix
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

# Search a 2D Matrix

Num: 74. Search a 2D Matrix

Link: https://leetcode.com/problems/search-a-2d-matrix/



## Desc

Write an efficient algorithm that searches for a value `target` in an `m x n` integer matrix `matrix`. This matrix has the following properties:

- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg)

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
Output: false
```

 

## Solution

### 1. Binary Search

##### 思路

- 将二维数组视作一位数组，因为整体是有序的
- 通过`/`,`%`确定在二维数组的位置



##### 解法 

```java
class Solution {
  
  
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix.length == 0) {
            return false;
        }
        int m = matrix.length;
        int n = matrix[0].length;
        int left = 0;
        int right = m * n - 1;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            int row = mid / n;
            int column = mid % n;
            int value = matrix[row][column];
            if (value == target) {
                return true;
            } else if (value > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
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

