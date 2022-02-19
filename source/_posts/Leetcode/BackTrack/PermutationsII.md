---
title: Permutations II
date: 
updated:
tags: BackTrack
categories: Leetcode
keywords: BackTrack, Permutations II
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

# Permutations II

Num: 47. Permutations II

Link: https://leetcode.com/problems/permutations/



## Desc

Given a collection of numbers, `nums`, that might contain duplicates, return *all possible unique permutations **in any order**.* 

**Example 1:**

```
Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**Example 2:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
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
- 去重（当前递归层循环内取值不能重复）



```java
class Solution {

    // 结果集合
    private final List<List<Integer>> result = new ArrayList<>();

    private int[] candidateValue;


    public List<List<Integer>> permuteUnique(int[] nums) {
        candidateValue = nums;
        boolean[] used = new boolean[nums.length];
        backtrack(new ArrayList<>(), used);
        return result;
    }

    private void backtrack(List<Integer> path, boolean[] used) {
        if (path.size() == candidateValue.length) {
            result.add(new ArrayList<>(path));
            return;
        }

        Set<Integer> set = new HashSet<>();

        for (int i = 0; i < candidateValue.length; i++) {
            if (used[i]) {
                continue;
            }

            int currentValue = candidateValue[i];
            System.out.println(currentValue);
            if (set.contains(currentValue)) {
                continue;
            } else {
                set.add(currentValue);
            }
            used[i] = true;

            path.add(currentValue);
            backtrack(path, used);
            path.remove(path.size() - 1);
            used[i] = false;
        }
    }


}
```



因为涉及到较多的`contains`判断，时间复杂度和空间复杂度都较高。

 

### 2. Backtrack 2

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
- 排序，过滤相邻重复元素。



```java
class Solution {


    // 结果集合
    private final List<List<Integer>> result = new ArrayList<>();

    private int[] candidateValue;


    public List<List<Integer>> permuteUnique(int[] nums) {
        candidateValue = nums;
        Arrays.sort(candidateValue);
        boolean[] used = new boolean[nums.length];
        backtrack(new ArrayList<>(), used);
        return result;
    }

    private void backtrack(List<Integer> path, boolean[] used) {
        if (path.size() == candidateValue.length) {
            result.add(new ArrayList<>(path));
            return;
        }

        for (int i = 0; i < candidateValue.length; i++) {
            if (used[i]) {
                continue;
            }
            // 从第二个元素开始，出现相同元素，且前一元素在本次递归过程中未被选中，则跳过。因为i=i-1时已完成选择。
            if (i > 0 && candidateValue[i - 1] == candidateValue[i] && !used[i - 1]) {
                continue;
            }
            int currentValue = candidateValue[i];

            //  选择
            used[i] = true;
            path.add(currentValue);
            backtrack(path, used);
            // 撤销选择
            path.remove(path.size() - 1);
            used[i] = false;
        }
    }


}
```



提交执行时间由60ms减少至4ms.



## Reference

[回溯算法套路详解] https://labuladong.github.io/algo/1/5/