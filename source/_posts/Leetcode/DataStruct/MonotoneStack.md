---
title: 单调栈 - Monotone Stack
date: 
updated:
tags: 单调栈
categories: 
  - Leetcode
  - 数据结构
  - 单调栈
keywords: 单调栈
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

# 单调栈 - Monotone Stack

## 什么是单调栈？

栈（stack）是很简单的一种数据结构，先进后出的逻辑顺序，符合某些问题的特点，比如说函数调用栈。

单调栈实际上就是栈，只是利用了一些巧妙的逻辑，使得每次新元素入栈后，栈内的元素都保持有序（单调递增或单调递减）。

单调栈用途不太广泛，只处理一种典型的问题，叫做 Next Greater Element。 

1. 单调递增栈就是从栈底到栈顶是从大到小
2. 单调递减栈就是从栈底到栈顶是从小到大



**时间复杂度**

分析它的时间复杂度，要从整体来看：总共有 `n` 个元素，每个元素都被 `push` 入栈了一次，而最多会被 `pop` 一次，没有任何冗余操作。所以总的计算规模是和元素规模 `n` 成正比的，也就是 `O(n)` 的复杂度。



## 典型题解

### 1. Next Greater Element I (下一个更大元素)

这道题是leetcode 496. Next Greater Element I。

#### 题目描述

The **next greater element** of some element `x` in an array is the **first greater** element that is **to the right** of `x` in the same array.

You are given two **distinct 0-indexed** integer arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

For each `0 <= i < nums1.length`, find the index `j` such that `nums1[i] == nums2[j]` and determine the **next greater element** of `nums2[j]` in `nums2`. If there is no next greater element, then the answer for this query is `-1`.

Return *an array* `ans` *of length* `nums1.length` *such that* `ans[i]` *is the **next greater element** as described above.*

 

**Example 1:**

```
Input: nums1 = [4,1,2], nums2 = [1,3,4,2]
Output: [-1,3,-1]
Explanation: The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
```

**Example 2:**

```
Input: nums1 = [2,4], nums2 = [1,2,3,4]
Output: [3,-1]
Explanation: The next greater element for each value of nums1 is as follows:
- 2 is underlined in nums2 = [1,2,3,4]. The next greater element is 3.
- 4 is underlined in nums2 = [1,2,3,4]. There is no next greater element, so the answer is -1.
```

#### 思路分析

这个是单调栈的典型题目。目的是寻找数组中比当前元素大的第一个元素。

最简单的思路是双层for循环，时间复杂度较高。采用单调栈可以在O(n)时间复杂度下解决问题。

- 从前往后遍历，将元素push到栈中，栈顶元素为最新元素。
- 根据当前元素的值，更新之前小于该元素的元素的结果值。（采用pop() 可以在O(1)复杂度依次获得栈中的符合要求的元素）
- 因为本题是要求子集的元素对应的更大元素，我们可以用一个map保存结果。如map中不存在，表明后面不存在比该元素大的值，应设为-1.

#### 题解

```java
class Solution {
  
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int[] ans = new int[nums1.length];
        Map<Integer, Integer> map = new HashMap<>();
        Stack<Integer> stack = new Stack<>();
				// 对nums2进行遍历
        for (int num : nums2) {
            while (!stack.isEmpty() && num > stack.peek()) {
                map.put(stack.pop(), num);
            }
						// 将当前元素加入栈
            stack.push(num);
        }
				
      	// 结果赋值
        for (int i = 0; i < nums1.length; i++) {
          	// 如map中不存在，表明后面不存在比该元素大的值，应设为-1.
            int target = map.getOrDefault(nums1[i], -1);
            ans[i] = target;
        }

        return ans;
    }
}
```



### 2. Next Greater Element II (环形数组中的下一个更大元素)

这道题是leetcode 503. Next Greater Element II.

#### 题目描述

Given a circular integer array `nums` (i.e., the next element of `nums[nums.length - 1]` is `nums[0]`), return *the **next greater number** for every element in* `nums`.

The **next greater number** of a number `x` is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return `-1` for this number.

 

**Example 1:**

```
Input: nums = [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2; 
The number 2 can't find next greater number. 
The second 1's next greater number needs to search circularly, which is also 2.
```

**Example 2:**

```
Input: nums = [1,2,3,4,3]
Output: [2,3,4,-1,4]
```



#### 思路分析

与上面一题的不同之处在于，本题的数组为循环数组。我们可以通过 % 运算符求模（余数），来获得环形特效。因此，我们可以通过`% length`模拟将数组加倍。

- For循环的次数变为2倍的数组长度，这样可以保证每个元素的解都被运算过。
- 仅将第一次遍历的元素入栈，可以防止重复计算，以及停止遍历后，残留元素的取值模糊问题。
- 遍历完毕后，栈中仍存在的元素即为“找不到更大元素”的值，置为-1.

#### 题解

```java
class Solution {
  
    public int[] nextGreaterElement(int[] nums) {
        int length = nums.length;
        int[] ans = new int[length];
        Stack<Integer> stack = new Stack<>();

        for (int i = 0; i < 2 * length - 1; i++) {
          	// 取模获取真实数组下标
            int current = i % length;
            while (!stack.isEmpty() && nums[current] > nums[stack.peek()]) {
                ans[stack.pop()] = nums[current];
            }
          	// 仅将第一次遍历的元素入栈，可以防止重复计算，以及停止遍历后，残留元素的取值模糊问题。
            if (i < length) {
                stack.push(current);
            }
        }
				// 遍历完毕后，栈中仍存在的元素即为“找不到更大元素”的值，置为-1.
        while (!stack.isEmpty()) {
            int index = stack.pop();
            ans[index] = -1;
        }

        return ans;
    }
    
}
```



### 3. Daily Temperatures (每日温度)

这道题是leetcode 739.



#### 题目描述

Given an array of integers `temperatures` represents the daily temperatures, return *an array* `answer` *such that* `answer[i]` *is the number of days you have to wait after the* `ith` *day to get a warmer temperature*. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

 

**Example 1:**

```
Input: temperatures = [73,74,75,71,69,72,76,73]
Output: [1,1,4,2,1,1,0,0]
```

**Example 2:**

```
Input: temperatures = [30,40,50,60]
Output: [1,1,1,0]
```

**Example 3:**

```
Input: temperatures = [30,60,90]
Output: [1,1,0]
```



#### 思路分析

与前面单调栈的思路一样，都是求第一个大于当前元素值的元素。只不过在这个问题中需要纪录的是两者索引下标的距离，也就是间隔天数。

因此，栈中应该存放数组下标。

#### 题解

```java
class Solution {
  
    public int[] dailyTemperatures(int[] temperatures) {
        int length = temperatures.length;
        // 结果数组
        int[] ans = new int[length];

        Stack<Integer> stack = new Stack<>();

        for (int i = 0; i < length; i++) {
            // 栈不为空时，说明存在待求解的元素。当栈顶元素小于当前元素，则当前元素为第一个比栈顶元素大的值
            while (!stack.isEmpty() && temperatures[stack.peek()] < temperatures[i]) {
                // 弹出已求解元素，并赋值到结果数组
                int index = stack.pop();
                ans[index] = i - index;
            }
            // 当前元素入栈
            stack.push(i);
        }

        return ans;
    }
}
```

