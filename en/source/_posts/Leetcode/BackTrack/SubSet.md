---
title: SubSet
date: 
updated:
tags: BackTrack
categories: Leetcode
keywords: BackTrack SubSet
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

# SubSet

Num: 78

Link: https://leetcode.com/problems/subsets/



## Desc

Given an integer array `nums` of **unique** elements, return *all possible subsets (the power set)*.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**Example 2:**

```
Input: nums = [0]
Output: [[],[0]]
```

## Solution

### 1. For Loop

```java
class Solution {
   public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        // 空集合
        result.add(new ArrayList<>());

        for (int i : nums) {
            // avoid ConcurrentModificationException
            List<List<Integer>> temp = new ArrayList<>();
            for (List<Integer> existList : result) {
                // 在原集合基础上 new 新集合
                List<Integer> newList = new ArrayList<>(existList);
                newList.add(i);
                temp.add(newList);
            }

            result.addAll(temp);
        }
        return result;
    }
 
}
```



### 2. BackTrack

```java
class Solution {
   public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        backtrack(result, path, nums, 0);
        return result;
    }

    private void backtrack(List<List<Integer>> result, List<Integer> path, int[] nums, int startOfSelect) {
        result.add(new ArrayList<>(path));
        // 选择范围
        for (int i = startOfSelect; i < nums.length; i++) {
            // 做选择，将当前节点添加至path
            path.add(nums[i]);
            // 递归，对子节点做选择
            backtrack(result, path, nums, i + 1);
            // 删除之前的选择，进行下一次
            path.remove(path.size() - 1);
        }
    }
 
}
```

 



## Reference

[回溯算法套路详解] https://labuladong.github.io/algo/1/5/
