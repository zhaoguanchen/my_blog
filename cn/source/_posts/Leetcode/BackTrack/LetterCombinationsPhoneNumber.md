---
title: Letter Combinations of a Phone Number
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

# Letter Combinations of a Phone Number

Num: 17

Link: https://leetcode.com/problems/letter-combinations-of-a-phone-number/



## Desc

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

**Example 1:**

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**Example 2:**

```
Input: digits = ""
Output: []
```

## Solution

### 1. For Loop

- 利用map建立字符映射，也可以使用数组。
- 进行遍历，左侧数字对应的字符集合放入集合中，后续依次追加新字符（a, ab, abc）
- 移除中间元素。

```java
class Solution {
    private static final Map<Character, String> map = new HashMap<>();

    static {
        map.put('2', "abc");
        map.put('3', "def");
        map.put('4', "ghi");
        map.put('5', "jkl");
        map.put('6', "mno");
        map.put('7', "pqrs");
        map.put('8', "tuv");
        map.put('9', "wxyz");

    }


    /**
     * for loop
     */
    public static List<String> letterCombinations(String digits) {
        LinkedList<String> ans = new LinkedList<>();
        if (digits.isEmpty()) return ans;
        ans.add("");
        while (!ans.isEmpty() && ans.peek().length() != digits.length()) {
            String remove = ans.remove();
            String currentValue = map.get(digits.charAt(remove.length()));
            for (char c : currentValue.toCharArray()) {
                ans.addLast(remove + c);
            }
        }
        return ans;
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
    private static final Map<Character, String> map = new HashMap<>();

    static {
        map.put('2', "abc");
        map.put('3', "def");
        map.put('4', "ghi");
        map.put('5', "jkl");
        map.put('6', "mno");
        map.put('7', "pqrs");
        map.put('8', "tuv");
        map.put('9', "wxyz");

    }

    LinkedList<String> result = new LinkedList<>();
    String select;

    public List<String> letterCombinations(String digits) {
        if (digits.isEmpty()) {
            return result;
        }
        StringBuilder path = new StringBuilder();
        Integer start = 0;
        select = digits;
        backtrack(path, start);
        return result;
    }

    private void backtrack(StringBuilder path, Integer start) {
        if (path.length() == select.length()) {
            result.add(path.toString());
            return;
        }

        char c = select.charAt(start);
        String values = map.get(c);
        for (char value : values.toCharArray()) {
            path.append(value);
            backtrack(path, start + 1);
            path.deleteCharAt(path.length() - 1);
        }

    }
}
```





## Reference

[回溯算法套路详解](https://zhuanlan.zhihu.com/p/93530380)