---
title: Generate Parentheses
date: 
updated:
tags: BackTrack
categories: Leetcode
keywords: BackTrack, Combinations
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

# Generate Parentheses

Num: 22. Generate Parentheses

Link: https://leetcode.com/problems/generate-parentheses/



## Desc

Given `n` pairs of parentheses, write a function to *generate all combinations of well-formed parentheses*.

 

**Example 1:**

```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

**Example 2:**

```
Input: n = 1
Output: ["()"]
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
  
    private static int targetValue;

    private static final List<List<Integer>> result = new ArrayList<>();

    private static int[] candidateValue;


    public static List<List<Integer>> combinationSum(int[] candidates, int target) {
        targetValue = target;
        candidateValue = candidates;
        Arrays.sort(candidateValue);
        backtrack(new ArrayList<>(), 0, 0);

        return result;

    }

    private static void backtrack(List<Integer> path, int sum, int start) {
        if (sum == targetValue) {
            result.add(new ArrayList<>(path));
            return;
        }
        if (sum > targetValue) {
            return;
        }

        for (int i = start; i < candidateValue.length; i++) {
            int currentValue = candidateValue[i];
            path.add(currentValue);
            sum += currentValue;
            backtrack(path, sum, i);
            path.remove(path.size() - 1);
            sum -= currentValue;
        }
    }

}
```



 

## Reference

[回溯算法套路详解]