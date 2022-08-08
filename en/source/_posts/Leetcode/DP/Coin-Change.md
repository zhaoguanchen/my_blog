---
title: Coin Change
date: 
updated:
tags: DP
categories: 
	- Leetcode
	- DP
keywords: DP
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

# Coin Change

Num: 322. Coin Change

Link: https://leetcode.com/problems/coin-change/



## Desc

Return *the fewest number of coins that you need to make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

 

**Example 1:**

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

**Example 2:**

```
Input: coins = [2], amount = 3
Output: -1
```

**Example 3:**

```
Input: coins = [1], amount = 0
Output: 0
```

 

## Solution

### 1. DP自顶向下（Map作为“备忘录”）

##### 思路

- 子问题为`amount - i`的解加一。
- 利用map存储已经计算过的节点。



##### 解法 

```java
class Solution {
  
    private final Map<Integer, Integer> memo = new HashMap<>();

    public int coinChange(int[] coins, int amount) {
        if (amount == 0) {
            return 0;
        }
        if (amount < 0) {
            return -1;
        }
        if (memo.containsKey(amount)) {
            return memo.get(amount);
        }
        int number = Integer.MAX_VALUE;

        for (int i : coins) {
            int subNumber = coinChange(coins, amount - i);
            if (subNumber == -1) {
                continue;
            }
            number = Math.min(number, subNumber + 1);
        }
        if (number == Integer.MAX_VALUE) {
            number = -1;
        }

        memo.put(amount, number);

        return number;

    }

}
```

   



### 2. DP自顶向下（Array作为“备忘录”）

##### 思路

- 子问题为`amount - i`的解加一。
- 利用数组存储已经计算过的节点  **相较于map更高效**。
  - Runtime: 24 ms, faster than 65.99% of Java online submissions for Coin Change.



##### 解法 

```java
class Solution {
  
    private static int[] memoArray;

    public static int coinChange(int[] coins, int amount) {
        // 采用数组作为"备忘录"，相较于map更高效
        memoArray = new int[amount + 1];
        Arrays.fill(memoArray, -2);
        return dpArrayAsMemo(coins, amount);
    }

    public static int dpArrayAsMemo(int[] coins, int amount) {
        if (amount == 0) {
            return 0;
        }
        // 无解
        if (amount < 0) {
            return -1;
        }

        // 初始化为-2
        if (memoArray[amount] != -2) {
            return memoArray[amount];
        }

        // 设为int最大值，便于比较
        int number = Integer.MAX_VALUE;

        for (int i : coins) {
            int subNumber = dpArrayAsMemo(coins, amount - i);
            // 子问题无解，跳过
            if (subNumber == -1) {
                continue;
            }
            // 找出最优解
            number = Math.min(number, subNumber + 1);
        }
        // 未找到最优解，无解
        if (number == Integer.MAX_VALUE) {
            number = -1;
        }

        memoArray[amount] = number;

        return number;

    }

}
```



### 3. DP自底向上（Array作为“备忘录”）

##### 思路

- 从最小子问题开始，以迭代方式逐步递增。
- 利用数组存储已经计算过的节点  **相较于map更高效**。

- 不存在堆栈调用，空间复杂度为O(1).

##### 解法 

```java
class Solution {
  
    public static int coinChangeBottomToUp(int[] coins, int amount) {
        // 采用数组作为"备忘录"，相较于map更高效
        int[] memoArray2 = new int[amount + 1];
        Arrays.fill(memoArray2, -2);
        memoArray2[0] = 0;
        for (int i = 1; i <= amount; i++) {
            int best = Integer.MAX_VALUE;
            for (int coin : coins) {
                // 不合法情形，跳过
                if (i - coin < 0) {
                    continue;
                }
                int subBest = memoArray2[i - coin];
                // 子问题无解
                if (subBest == -1) {
                    continue;
                }
                best = Math.min(best, subBest);
            }
            if (best == Integer.MAX_VALUE) {
                memoArray2[i] = -1;
            } else {
                memoArray2[i] = best + 1;
            }
        }

        return memoArray2[amount];
    }

}
```





## Summary

- 递归问题主要有两种方式，自顶向下和自底向上，分别对应着递归和迭代方法。

- “备忘录”可以采用HashMap和数组，数组效率更高。追求faster时应采用数组。

  



## Reference

[动态规划解题套路框架](https://labuladong.github.io/algo/1/4/)