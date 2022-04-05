本文包含两数之和（Two Sum）、三数之和（3Sum）、四数之和（4Sum）以及更通用的n数之和。

# 1. 两数之和（Two Sum)

LeetCode第一题即为[Two Sum][https://leetcode.com/problems/two-sum/]. 求数组中和为目标值的两个数的索引下标。

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

LeetCode第15题即为[3 Sum][https://leetcode.com/problems/3sum/]. 求数组中和为0的三个数的组合，同时三数之间互不相等，返回的组合也不能重复。

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

### 1. 排序后双指针

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



## 

# 3. 四数之和（4Sum）







# 4. n数之和