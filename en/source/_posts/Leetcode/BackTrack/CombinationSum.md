---
title: Combination Sum
date: 
updated:
tags: BackTrack
categories: Leetcode
keywords: BackTrack, Combinations, Sum
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

# Combination Sum

Num: 39. Combination Sum

Link: https://leetcode.com/problems/combination-sum/



## Desc

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is **guaranteed** that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

 

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```

**Example 3:**

```
Input: candidates = [2], target = 1
Output: []
```



## Solution

### 1. Backtrack

思路：

```
def backtrack(...):
    for 选择 in 选择列表:
        做选择
        backtrack(...)
        撤销选择
```



```java
class Solution {
  
    private int targetValue;

    private final List<List<Integer>> result = new ArrayList<>();
    // 结果集合
    private int[] candidateValue;


    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        targetValue = target;
        candidateValue = candidates;
        backtrack(new ArrayList<>(), 0, 0);

        return result;

    }

    private void backtrack(List<Integer> path, int sum, int start) {
        // 结束条件：找到和等于目标值的组合
        if (sum == targetValue) {
            result.add(new ArrayList<>(path));
            return;
        }
        // 超出目标值
        if (sum > targetValue) {
            return;
        }

        for (int i = start; i < candidateValue.length; i++) {
            int currentValue = candidateValue[i];
            // 选择
            path.add(currentValue);
            sum += currentValue;
            // 回溯，仅需要考虑当前位及以后位，若考虑之前位会造成重复。
            backtrack(path, sum, i);
            // 撤销选择
            path.remove(path.size() - 1);
            sum -= currentValue;
        }
    }

}
```



 

## Reference

[回溯算法套路详解] https://labuladong.github.io/algo/1/5/