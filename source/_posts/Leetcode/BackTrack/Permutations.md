---
title: Permutations
date: 
updated:
tags: BackTrack
categories: Leetcode
keywords: BackTrack, Permutations
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

# Permutations

Num: 46. Permutations

Link: https://leetcode.com/problems/permutations/



## Desc

Given an array `nums` of distinct integers, return *all the possible permutations*. You can return the answer in **any order**.

 

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**Example 2:**

```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```

**Example 3:**

```
Input: nums = [1]
Output: [[1]]
```

 

## Solution

### 1. Backtrack

##### 思路

```
def backtrack(...):
    for 选择 in 选择列表:
        做选择
        backtrack(...)
        撤销选择
```



##### 全排列

- 非已有节点都要考虑进去



```java
class Solution {


    // 结果集合
    private final List<List<Integer>> result = new ArrayList<>();

    private int[] candidateValue;

    public List<List<Integer>> permute(int[] nums) {
        candidateValue = nums;
        backtrack(new ArrayList<>());
        return result;
    }

    private void backtrack(List<Integer> path) {
        // 结束条件：长度相等
        if (path.size() == candidateValue.length) {
            result.add(new ArrayList<>(path));
            return;
        }
        
        for (int i = 0; i < candidateValue.length; i++) {
            int currentValue = candidateValue[i];
            // 排除已存在的元素
            if (path.contains(currentValue)) {
                continue;
            }

            // 选择
            path.add(currentValue);
            // 回溯，考虑所有候选项
            backtrack(path);
            // 撤销选择
            path.remove(path.size() - 1);
        }
    }


}
```



 

## Reference

[回溯算法套路详解] https://labuladong.github.io/algo/1/5/