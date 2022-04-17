---
title: 解决一系列Sum问题
date: 
updated:
tags: nSum
categories: 
  - Leetcode
  - nSum
keywords: nSum
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



本文包含两数之和（Two Sum）、三数之和（3Sum）、四数之和（4Sum）以及更通用的n数之和。

# 1. 两数之和（Two Sum)

LeetCode第一题即为[Two Sum](https://leetcode.com/problems/two-sum/). 求数组中和为目标值的两个数的索引下标。

## 题目描述

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have **exactly one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Example 2:**

```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**

```
Input: nums = [3,3], target = 6
Output: [0,1]
```



## 解法

 由于要返回的是数组下标，因此我们不能对数组进行排序。而利用HashMap缓存元素则是比较好的解决方法。



```java
class Solution {
  
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> memo = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int temp = target - nums[i];
            if (memo.containsKey(temp)) {
                return new int[]{memo.get(temp), i};
            }

            memo.put(nums[i], i);
        }

        return null;

    }
}
```



## 扩展1 --返回数值

如果题目要求返回的不是索引，而是元素值，我们可以通过双指针的方法从两端相向而行得到答案。双指针解法的前提是数组是有序的，因此我们需要先对数组进行原地排序。

```java
class Solution {
  
    private int[] twoSum(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while (left < right) {
            int leftVal = nums[left];
            int rightVal = nums[right];
            int sum = leftVal + rightVal;
            if (sum < target) {
					      left++;
            } else if (sum > target) {
			          right--;
            } else {
                return new int[]{leftVal, rightVal};
            }
        }

        return null;
    }

}
```



## 扩展2 --返回所有结果

原题明确说明只有一个符合条件的结果，假设存在多个结果呢？

我们需要定义`List<List<Integer>>`来接收所有的结果。而搜索到符合条件的值时，不再直接返回，而是添加到结果列表中后继续寻找。

```java
class Solution {
  
    private List<List<Integer>> twoSum(int[] nums, int target) {
       			List<List<Integer>> ans = new ArrayList<>();

            int left = start;
            int right = nums.length - 1;
            while (left < right) {
                int leftVal = nums[left];
                int rightVal = nums[right];
                int sum = leftVal + rightVal;
                if (sum < target) {
                    while (left < right && nums[left] == leftVal) {
                        left++;
                    }
                } else if (sum > target) {
                    while (left < right && nums[right] == rightVal) {
                        right--;
                    }
                } else {
                    List<Integer> subRes = Arrays.asList(nums[left], nums[right]);
                    ans.add(subRes);
                    while (left < right && nums[left] == leftVal) {
                        left++;
                    }
                    while (left < right && nums[right] == rightVal) {
                        right--;
                    }
                }
            }
            return ans;
    }

}
```

需要注意的是，代码中出现了多次类似`while (left < right && nums[left] == leftVal) { left++;}`、`while (left < right && nums[right] == rightVal) { right--;}`的结构，此举是为了去除重复元素。



这样，一个通用化的 `twoSum` 函数就写出来了。后面解决 `3Sum` 和 `4Sum` 的时候会复用这个函数。

这个函数的时间复杂度非常容易看出来，双指针操作的部分虽然有那么多 while 循环，但是时间复杂度还是 `O(N)`，而排序的时间复杂度是 `O(NlogN)`，所以这个函数的时间复杂度是 `O(NlogN)`。



# 2. 三数之和（3Sum）

LeetCode第15题即为[3 Sum](https://leetcode.com/problems/3sum/). 求数组中和为0的三个数的组合，同时三数之间互不相等，返回的组合也不能重复。

## 题目描述

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```

**Example 2:**

```
Input: nums = []
Output: []
```

**Example 3:**

```
Input: nums = [0]
Output: []
```

 

## 解法

### 1. HashTable

首先我们可以采用Hash的方法，缓存已经遍历过的元素，以此降低时间复杂度。

```java
class Solution {
  
    /**
     * Hash Table
     *
     * @param nums
     * @return
     */
    public List<List<Integer>> threeSumNoSort(int[] nums) {
        // 结果set，方便去重
        Set<List<Integer>> res = new HashSet<>();
        // 记录在i循环下，某数是否出现过
        Map<Integer, Integer> memo = new HashMap<>();
        // 判断num[i]的值是否之前存在过，如果是，则处理结果必定重复
        Set<Integer> existed = new HashSet<>();
        for (int i = 0; i < nums.length - 2; i++) {
            // 当前值已处理过
            if (existed.contains(nums[i])) {
                continue;
            }
            existed.add(nums[i]);
            for (int j = i + 1; j < nums.length; j++) {
                int temp = -nums[i] - nums[j];
                // 如果差值之前出现过，则可以组成结果对
                if (memo.containsKey(temp)) {
                    if (memo.get(temp) == i) {
                        List<Integer> tuple = Arrays.asList(nums[i], nums[j], temp);
                        // 排序以去重
                        tuple.sort(Comparator.comparingInt(value -> value));
                        res.add(tuple);
                    }
                }
                // 当前在i循环下，nums[j] 出现
                memo.put(nums[j], i);
            }
        }
      
        return new ArrayList<>(res);
    }
}
```

### 2. 排序后双指针

与两数之和类似，同样可以通过双指针来解决三数之和，只不过在双指针的基础上多一层for循环。

```java
    /**
     * 排序后双指针法
     *
     * @param nums
     * @return
     */
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length && nums[i] <= 0; i++) {
            // 数字重复，直接跳过
            if (i != 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            // 在后面的序列中使用双指针
            int left = i + 1;
            int right = nums.length - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum < 0) {
                    // 跳过重复元素
                    while (left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }
                    left++;
                } else if (sum > 0) {
                    // 跳过重复元素
                    while (left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }
                    right--;
                } else {
                    // 找到结果
                    ans.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    // 跳过重复元素
                    while (left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }
                    left++;
                    // 跳过重复元素
                    while (left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }
                    right--;
                }
            }
        }

        return ans;
    }

```

**关键点在于，不能让第一个数重复，至于后面的两个数，我们复用的 `twoSum` 函数会保证它们不重复**。所以代码中需要用一个判断来保证 `3Sum` 中第一个元素不重复。

至此，`3Sum` 问题就解决了，时间复杂度不难算，排序的复杂度为 `O(NlogN)`，`twoSumTarget` 函数中的双指针操作为 `O(N)`，`threeSumTarget` 函数在 for 循环中调用 `twoSumTarget` 所以总的时间复杂度就是 `O(NlogN + N^2) = O(N^2)`。

# 2. 三数之和（3Sum Closest）

有了三数之和，我们再来看它的小变种。

LeetCode第16题为[3 Sum Closest](https://leetcode.com/problems/3sum-closest/). 求数组中和离目标值最近的三个数的组合，简单之处在于题目指定仅有一个答案。

## 题目描述

Given an integer array `nums` of length `n` and an integer `target`, find three integers in `nums` such that the sum is closest to `target`.

Return *the sum of the three integers*.

You may assume that each input would have exactly one solution.

 

**Example 1:**

```
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

**Example 2:**

```
Input: nums = [0,0,0], target = 1
Output: 0
```

  

## 解法

### 1. 排序后双指针

我们对上面三数之和的解法进行略微改进，即可得到本题的答案。

整体框架不变，依然是采用外层循环，内层双指针的方法。区别在于我们定义了一个变量`closest`来存储最小值，定义了变量`minDiff`来存储最小的绝对差值。这样，我们只需要在每次判断sum与target的绝对差是否更小就可以了。

如果sum恰好等于target，则可以直接返回sum，因为没有更小的差值了。

```java
    public int threeSumClosest(int[] nums, int target) {
        int closest = 0;
        int minDiff = Integer.MAX_VALUE;
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            if (0 != i && nums[i] == nums[i - 1]) {
                continue;
            }

            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {
                int leftVal = nums[left];
                int rightVal = nums[right];
                int sum = nums[i] + leftVal + rightVal;
                int diff = Math.abs(sum - target);
                if (diff < minDiff) {
                    minDiff = diff;
                    closest = sum;
                }


                if (sum < target) {
                    while (left < right && nums[left] == leftVal) {
                        left++;
                    }
                } else if (sum > target) {
                    while (left < right && nums[right] == rightVal) {
                        right--;
                    }
                } else {
                    return target;
                }
            }

        }

        return closest;
    }


```

**关键点在于，不能让第一个数重复，至于后面的两个数，我们复用的 `twoSum` 函数会保证它们不重复**。所以代码中需要用一个判断来保证 `3Sum` 中第一个元素不重复。

至此，`3Sum` 问题就解决了，时间复杂度不难算，排序的复杂度为 `O(NlogN)`，`twoSumTarget` 函数中的双指针操作为 `O(N)`，`threeSumTarget` 函数在 for 循环中调用 `twoSumTarget` 所以总的时间复杂度就是 `O(NlogN + N^2) = O(N^2)`。



# 3. 四数之和（4Sum）

LeetCode第18题即为[4 Sum](https://leetcode.com/problems/4sum/). 求数组中和为target的四个数的组合，同时返回的组合也不能重复。

## 题目描述

Given an array `nums` of `n` integers, return *an array of all the **unique** quadruplets* `[nums[a], nums[b], nums[c], nums[d]]` such that:

- `0 <= a, b, c, d < n`
- `a`, `b`, `c`, and `d` are **distinct**.
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in **any order**.

 

**Example 1:**

```
Input: nums = [1,0,-1,0,-2,2], target = 0
Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

**Example 2:**

```
Input: nums = [2,2,2,2,2], target = 8
Output: [[2,2,2,2]]
```

## 解法

###  1. 排序后双指针

`4Sum` 完全就可以用跟上面相同的思路：穷举第一个数字，然后调用 `3Sum` 函数计算剩下三个数，最后组合出和为 `target` 的四元组。

为了看起来方便，我们在解法里分别实现了twoSum, threeSum 和 fourSum。

```java
    // 全局变量
    private int[] data;

    /**
     * 两数之和
     *
     * @param start
     * @param end
     * @param target
     * @return
     */
    private List<List<Integer>> twoSum(int start, int end, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        int left = start;
        int right = end;
        while (left < right) {
            int sum = data[left] + data[right];
            if (sum < target) {
                left++;
                while (left < right && data[left] == data[left - 1]) {
                    left++;
                }
            } else if (sum > target) {
                right--;
                while (left < right && data[right] == data[right + 1]) {
                    right--;
                }
            } else {
                List<Integer> subRes = Arrays.asList(data[left], data[right]);
                ans.add(subRes);
                left++;
                while (left < right && data[left] == data[left - 1]) {
                    left++;
                }
                right--;
                while (left < right && data[right] == data[right + 1]) {
                    right--;
                }
            }
        }

        return ans;
    }

    /**
     * 三数之和
     *
     * @param start
     * @param end
     * @param target
     * @return
     */
    private List<List<Integer>> threeSum(int start, int end, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        for (int i = start; i <= end; i++) {
            // 去重
            if (i != start && data[i] == data[i - 1]) {
                continue;
            }
            int cur = data[i];
            int subTarget = target - cur;
            // 调用两数之和计算子结果
            List<List<Integer>> twoSum = twoSum(i + 1, end, subTarget);

            for (List<Integer> item : twoSum) {
                List<Integer> subRes = new ArrayList<>(item);
                subRes.add(cur);
                ans.add(subRes);
            }
        }

        return ans;
    }


    /**
     * 四数之和
     *
     * @param nums
     * @param target
     * @return
     */
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(nums);
        data = nums;

        for (int i = 0; i < data.length; i++) {
            // 去重
            if (i != 0 && data[i] == data[i - 1]) {
                continue;
            }
            int cur = data[i];
            int subTarget = target - cur;
            // 调用三数之和计算子结果
            List<List<Integer>> threeSum = threeSum(i + 1, data.length - 1, subTarget);

            // 加上nums[i]组合结果
            for (List<Integer> item : threeSum) {
                List<Integer> subRes = new ArrayList<>(item);
                subRes.add(cur);
                ans.add(subRes);
            }
        }

        return ans;
    }
```

时间复杂度的分析和之前类似，for 循环中调用了 `threeSumTarget` 函数，所以总的时间复杂度就是 `O(N^3)`。

# 4. n数之和

在 LeetCode 上，`4Sum` 就到头了，**但是回想刚才写 `3Sum` 和 `4Sum` 的过程，实际上是遵循相同的模式的**。

因此我们可以抽象出一个统一的方法来处理n个数的和。

## 解法

函数声明为`private List<List<Integer>> nSum(int[] nums, int start, int target, int k) `，其中，k是需要计算和的数字的数量（k=4时为四数之和）；`start`为本层在数组中的起始位置；`target`为在本层要计算的目标值。



`sum()`为方法入口，我们在递归调用`nSum()`前进行了排序，避免在每层`nSum()`时都要进行没必要的排序。

当k==2时进入我们的base case，利用双指针法求解。

当k>2时，则需要递归调用`nSum()`求解子序列的值，然后加上nums[i]组合出最终的结果。

```java
    public List<List<Integer>> sum(int[] nums, int target) {
        // 排序放在递归调用外层，避免每次都排序
        Arrays.sort(nums);
        return nSum(nums, 0, target, 4);
    }


    private List<List<Integer>> nSum(int[] nums, int start, int target, int k) {
        List<List<Integer>> ans = new ArrayList<>();
        // If we have run out of numbers to add, return res.
        if (start == nums.length) {
            return ans;
        }

        // There are k remaining values to add to the sum. The
        // average of these values is at least target / k.
        int average_value = target / k;

        // We cannot obtain a sum of target if the smallest value
        // in nums is greater than target / k or if the largest
        // value in nums is smaller than target / k.
        if (nums[start] > average_value || average_value > nums[nums.length - 1]) {
            return ans;
        }

        // base 两数之和 双指针法
        if (k == 2) {
            int left = start;
            int right = nums.length - 1;
            while (left < right) {
                int leftVal = nums[left];
                int rightVal = nums[right];
                int sum = leftVal + rightVal;
                if (sum < target) {
                    while (left < right && nums[left] == leftVal) {
                        left++;
                    }
                } else if (sum > target) {
                    while (left < right && nums[right] == rightVal) {
                        right--;
                    }
                } else {
                    List<Integer> subRes = Arrays.asList(nums[left], nums[right]);
                    ans.add(subRes);
                    while (left < right && nums[left] == leftVal) {
                        left++;
                    }
                    while (left < right && nums[right] == rightVal) {
                        right--;
                    }
                }
            }
            return ans;
        }


        for (int i = start; i < nums.length; i++) {
            // 去重
            if (i != start && nums[i] == nums[i - 1]) {
                continue;
            }

            int cur = nums[i];
            int subTarget = target - cur;
            // 递归调用获得子序列
            List<List<Integer>> subSum = nSum(nums, i + 1, subTarget, k - 1);

            // 加上nums[i]组合结果
            for (List<Integer> item : subSum) {
                List<Integer> subRes = new ArrayList<>(item);
                subRes.add(cur);
                ans.add(subRes);
            }
        }

        return ans;

    }
```



值得注意的是，在这段代码中，我们针对某些不可能情况进行了判断。如果当前剩余数组中最大值小于k数之和的平均数，或者最小值大于k数之和的平均数，此时是不可能有结果的，直接返回空数组。

这虽然不能降低时间复杂度，但仍然可以显著的降低运行时间。

```java
        // If we have run out of numbers to add, return res.
        if (start == nums.length) {
            return ans;
        }

        // There are k remaining values to add to the sum. The
        // average of these values is at least target / k.
        int average_value = target / k;

        // We cannot obtain a sum of target if the smallest value
        // in nums is greater than target / k or if the largest
        // value in nums is smaller than target / k.
        if (nums[start] > average_value || average_value > nums[nums.length - 1]) {
            return ans;
        }


```





# 5. 总结

本文我们从二数之和开始，逐步扩展到了四数之和，并基于以上情况，总结出了可以计算n数之和的框架。