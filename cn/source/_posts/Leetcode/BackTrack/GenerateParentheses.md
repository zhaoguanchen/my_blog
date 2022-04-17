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

### 1. Brute Force

- 生成所有结果，根据规则判断是否正确。

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> combinations = new ArrayList();
        generateAll(new char[2 * n], 0, combinations);
        return combinations;
    }

    public void generateAll(char[] current, int pos, List<String> result) {
        if (pos == current.length) {
            if (valid(current))
                result.add(new String(current));
        } else {
            current[pos] = '(';
            generateAll(current, pos+1, result);
            current[pos] = ')';
            generateAll(current, pos+1, result);
        }
    }

    public boolean valid(char[] current) {
        int balance = 0;
        for (char c: current) {
            if (c == '(') balance++;
            else balance--;
            if (balance < 0) return false;
        }
        return (balance == 0);
    }
}
```

### 2. Backtrack

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
  
    private static int max;
    private final static List<String> list = new ArrayList<>();

    public static List<String> generateParenthesis(int n) {
        max = n;
        backtrack(list, new StringBuilder(), 0, 0);
        return list;
    }


    public static void backtrack(List<String> ans, StringBuilder cur, int open, int close) {
        // 当到达最终长度时，停止递归
        if (cur.length() == max * 2) {
            ans.add(cur.toString());
            return;
        }
        // 左括号数量小于结果长度一半时，可继续添加左括号
        if (open < max) {
            // 做选择
            cur.append("(");
            backtrack(ans, cur, open + 1, close);
            // 撤销
            cur.deleteCharAt(cur.length() - 1);
        }
        // 右括号数量小于左括号数量时，可以添加右括号
        if (close < open) {
            // 做选择
            cur.append(")");
            backtrack(ans, cur, open, close + 1);
            // 撤销
            cur.deleteCharAt(cur.length() - 1);
        }
    }

}
```





## Reference

[回溯算法套路详解]