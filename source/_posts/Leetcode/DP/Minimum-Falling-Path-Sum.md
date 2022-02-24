---
title: Minimum Falling Path Sum
date: 
updated:
tags: DP
categories: 
  - Leetcode
  - DP
keywords: DP | Path
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

# Minimum Falling Path Sum

Num: 931. Minimum Falling Path Sum

Link: https://leetcode.com/problems/minimum-falling-path-sum/



## Desc

Given an `n x n` array of integers `matrix`, return *the **minimum sum** of any **falling path** through* `matrix`.

A **falling path** starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. Specifically, the next element from position `(row, col)` will be `(row + 1, col - 1)`, `(row + 1, col)`, or `(row + 1, col + 1)`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/11/03/failing1-grid.jpg)

```
Input: matrix = [[2,1,3],[6,5,4],[7,8,9]]
Output: 13
Explanation: There are two falling paths with a minimum sum as shown.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/11/03/failing2-grid.jpg)

```
Input: matrix = [[-19,57],[-40,-5]]
Output: -59
Explanation: The falling path with a minimum sum is shown.
```

**Constraints:**

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 100`
- `-100 <= matrix[i][j] <= 100`

   



## Solution

### 1. 迭代法DP

##### 思路

- 某节点的路径依赖其左上方，上方，右上方节点的路径值，因此子问题为上方节点。
- base case为第一行数据本身。



##### 解法 

```java
class Solution {
  
  
    // 数据
    private int[][] data;
    // 备忘录 存储各节点的最小路径
    private int[][] memo;
    // 备忘录初始化默认值  不能初始化为0 因为数据可能为零
    private final int specificDefault = 99999;

    public int minFallingPathSum(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        data = matrix;
        // 初始化memo
        memo = new int[m][n];
        for (int i = 0; i < n; i++) {
            Arrays.fill(memo[i], specificDefault);
        }
        // 对最下行求最值
        int res = Integer.MAX_VALUE;
        for (int i = 0; i < n; i++) {
            res = Math.min(res, dp(m - 1, i));
        }
        return res;

    }

    private int dp(int i, int j) {
        // 边界检查，防止数组越界。可直接设置为最大值
        if (i < 0 || j < 0 || i > data.length - 1 || j > data[0].length - 1) {
            return Integer.MAX_VALUE;
        }
        // 第一层的最小路径为数据值，同时赋值到memo
        if (i == 0) {
            memo[i][j] = data[i][j];
            return memo[i][j];
        }

        // 检查备忘录
        if (memo[i][j] != specificDefault) {
            return memo[i][j];
        }

        // 递归求解  子问题为当前节点的左上，上，右上节点
        memo[i][j] = data[i][j] + Math.min(dp(i - 1, j - 1), Math.min(dp(i - 1, j), dp(i - 1, j + 1)));

        return memo[i][j];
    }  
}
```

   

## Summary

- 小问题推出大问题，找到递推关系
- 利用数组，map存储已计算节点





## Reference

[BASE CASE 和备忘录的初始值怎么定？](https://labuladong.github.io/algo/3/23/74/)

