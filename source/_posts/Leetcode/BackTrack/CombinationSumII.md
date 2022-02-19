---
title: Combination Sum II
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

# Combination Sum II

Num: 40. Combination Sum II

Link: https://leetcode.com/problems/combination-sum-ii/



## Desc

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

 

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

**Example 2:**

```
Input: candidates = [2,5,2,1,2], target = 5
Output: 
[
[1,2,2],
[5]
]
```

 

## Solution

### 1. Backtrack

##### 思路：

```
def backtrack(...):
    for 选择 in 选择列表:
        做选择
        backtrack(...)
        撤销选择
```



##### 跟1有什么不同？

- 给出的候选列表包含重复数字

需要考虑如何排除重复

##### 如何去重？

- 递归时从下一位开始（这样子序列不会包含本节点）
- 排序后，相同数字会相邻。for循环遇到相同数字时，直接跳过（因为该数字已经递归过了，没有必要再跑一遍）



```java
class Solution {
  
    private int targetValue;

    private final List<List<Integer>> result = new ArrayList<>();
    // 结果集合
    private int[] candidateValue;


    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        targetValue = target;
        candidateValue = candidates;
        // 排序
        Arrays.sort(candidateValue);
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
            // 去重判断
            if (i > start && currentValue == candidateValue[i - 1]) {
                continue;
            }

            // 选择
            path.add(currentValue);
            sum += currentValue;
            // 回溯，仅需要考虑当前位及以后位，若考虑之前位会造成重复。
            backtrack(path, sum, i + 1);
            // 撤销选择
            path.remove(path.size() - 1);
            sum -= currentValue;
        }
    }

}
```



 

## Reference

[回溯算法套路详解] https://labuladong.github.io/algo/1/5/