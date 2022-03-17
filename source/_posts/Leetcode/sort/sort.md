---
title: 常见排序算法
date: 
updated:
tags: 排序
categories: 
  - Leetcode
  - 排序
keywords: 排序
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

# 常见排序算法

测试这些算法的正确性，可以用 [Leetcode 912. Sort an Array](https://leetcode.com/problems/sort-an-array/). 也可以在[GeeksforGeeks](https://www.geeksforgeeks.org/)中找到对应题型进行提交。



## 1. 快速排序

快速排序的核心是找到分割点 -- pivot，使得左边的元素都小于pivot所在的元素，右边的元素都大于pivot所在的元素。

因此，`partition()`的实现至关重要，而其难点是各种边界问题的处理，稍有不慎满盘皆输。

对于快速排序，有多种代码版本可供选择，下面只介绍我觉得简单的版本。

### 版本1

我们且称之为right-base版。

递归调用过程大同小异，不再赘述。

`partition()`的实现过程为：

- 选择最右元素right为基准点，设为base
- 从左往右进行for循环，遇到比base小的，则与index进行交换。同时index后移。
- 结束循环后，array[index]为第一个不小于base的元素，与array进行交换，则此时index左侧均小于base，右侧均大于base。
- 完成后，index所在的位置值为base, 即为分界点。



```java
public class QuickSort {
 
    /**
     * 快速排序调用层
     * 找到分隔点pivot，使左侧小于base，右侧大于base。
     * 递归处理左右子数组。
     *
     * @param array
     * @param left
     * @param right
     */
    public void quickSort(int[] array, int left, int right) {
        if (left >= right) {
            return;
        }
        // find pivot element such that
        // elements smaller than pivot are on the left
        // elements greater than pivot are on the right
        int pivot = partition(array, left, right);
        // recursive call on the left of pivot
        quickSort(array, left, pivot - 1);
        // recursive call on the right of pivot
        quickSort(array, pivot + 1, right);

    }

    /**
     * 分隔数组，返回分界点
     * 选择right为基准点，设为base
     * 从左往右进行for循环，遇到比base小的，则与index进行交换。
     * index后移。
     * 结束循环后，array[index]为第一个不小于base的元素，与array进行交换，则index左侧均小于base，右侧均大于base。
     *
     * @param array
     * @param left
     * @param right
     */
    public int partition(int[] array, int left, int right) {
        // choose the rightmost element as pivot
        int pivot = array[right];
        // pointer for greater element
        int index = left;
        // traverse through all elements
        // compare each element with pivot
        for (int j = left; j < right; j++) {
          	// < or <=, it does matter
            if (array[j] <= pivot) {
                // if element smaller than pivot is found
                // swap it with the greater element pointed by i
                // swapping element at i with element at j
                swap(array, index, j);
                index++;
            }
        }

        // swap the pivot element with the greater element specified by i
        swap(array, index, right);
        // return the position from where partition is done
        return (index);
    }

    /**
     * 交换节点
     *
     * @param array
     * @param left
     * @param right
     */
    public void swap(int[] array, int left, int right) {
        int temp = array[left];
        array[left] = array[right];
        array[right] = temp;
    }


}

```





### 版本2

我们且称之为left-base版。这个版本是我第一次学习快速排序时所见到的最流行的版本，奇怪的是后来渐渐的在各大博客销声匿迹了。

 

`partition()`的实现过程为：

- 选择left为基准点，设为base
- 先从右边元素开始，找到第一个比base小的，将array[i]设为该值(array[j])。（第一次赋值，实际上是覆盖了base的值）
- 再从左边开始，找到第一个比base大的，将array[j]设为该值(array[i])。此时相当于完成了一次`左侧大元素`与`右侧小元素`的互换。
- 结束循环后，将array[i]的值设为base。此时，i即为分界点的坐标。

```java
public class QuickSort {

    /**
     * 传统方法
     * 选择left为基准点
     *
     * @param array
     * @param left
     * @param right
     */
    public void quickSort(int[] array, int left, int right) {
        // 元素个数小于等于1，无需排序，结束
        if (left >= right) {
            return;
        }

        int index = getPartition(array, left, right);
        quickSort(array, left, index - 1);
        quickSort(array, index + 1, right);
    }


    /**
     * 传统方法
     * 选择left为基准点，设为base
     * 先从右边元素开始，找到第一个比base小的，将array[i]设为该值(array[j])。
     * 再从左边开始，找到第一个比base大的，将array[j]设为该值(array[i])。
     * 结束循环后，将array[i]的值设为base。
     *
     * @param array
     * @param left
     * @param right
     */
    private int getPartition(int[] array, int left, int right) {
        int i = left, j = right;
        int baseVal = array[left];

        while (i < j) {
            while (i < j && array[j] >= baseVal) {
                j--;
            }
            array[i] = array[j];
            while (i < j && array[i] <= baseVal) {
                i++;
            }
            array[j] = array[i];
        }

        array[i] = baseVal;
        // i 为基准点坐标
        return i;
    }
 

}

```

### 补充：shuffle

为了使数组元素分布更加均匀，避免最坏情况，可以在排序前进行洗牌。

在leetcode中，洗牌能有效的提高case的执行速度。

```java
    /**
     * 洗牌
     *
     * @param nums
     */
    private void shuffle(int[] nums) {
        Random rand = new Random();
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            // 生成 [i, n - 1] 的随机数
            int r = i + rand.nextInt(n - i);
            swap(nums, i, r);
        }
    }
```



## 2. 快速选择

在计算机科学中，快速选择算法主要是用于在未排序的数组中找到第 k 个最小/大数字的算法。它的方法和快速排序算法类似，快速排序算法和快速选择选择算法都是由 Tony Hoare 发明。

### Top K 问题

在未排序的数组中找到第 **k** 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

**示例 1:**

```text
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```



以Leetcode 寻找第k大元素这一题为例，除了可以采用优先队列（最大堆）来实现，还可以采用快速选择算法。

相比于快速排序算法，我们只需要针对可能存在k的区间进行递归处理，因此其时间复杂度为O(n) [1/2 + 1/4 + 1/8 ····]。

同样的，通过洗牌（shuffle(int[] nums) ）可以避免最快情况的发生。



快速选择算法代码如下：

```java
package leetcode.solution.array;

import java.util.PriorityQueue;
import java.util.Random;

/**
 * 215. Kth Largest Element in an Array
 */
public class KthLargestElementArray {

    private int[] array;

    private int target;

    public int findKthLargest(int[] nums, int k) {
        target = nums.length - k;
        array = nums;
        // 打乱原数组，避免极端情况
        // 在Leetode提交中执行时间有显著提高
        shuffle(array);

        return helper(0, nums.length - 1);
    }


    private int helper(int left, int right) {
        int ans;
        if (left >= right) {
            return array[left];
        }

        int pivot = getPartition(left, right);
        if (pivot < target) {
            ans = helper(pivot + 1, right);
        } else if (pivot > target) {
            ans = helper(left, pivot - 1);
        } else {
            ans = array[pivot];
        }
        return ans;
    }

    private int getPartition(int left, int right) {
        int baseVal = array[right];
        int index = left;

        for (int i = left; i < right; i++) {
            if (array[i] < baseVal) {
                swap(i, index);
                index++;
            }
        }

        swap(index, right);

        return index;
    }

    private void swap(int left, int right) {
        int temp = array[left];
        array[left] = array[right];
        array[right] = temp;
    }
 
  
    /**
     * 洗牌
     *
     * @param nums
     */
    private void shuffle(int[] nums) {
        Random rand = new Random();
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            // 生成 [i, n - 1] 的随机数
            int r = i + rand.nextInt(n - i);
            swap(i, r);
        }
    }
}

```



优先队列（堆排序）的解法如下：

```java

    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> priorityQueue = new PriorityQueue<>();

        for (int i : nums) {
            priorityQueue.add(i);
            if (priorityQueue.size() > k) {
                priorityQueue.poll();
            }
        }

        return priorityQueue.peek();
    }

```

时间复杂度为O(n·logk),即循环次数*添加元素时堆调整的时间, 空间复杂度为队列的大小O(k).



 



## 3. 归并排序

归并排序的核心思想是将数组分为左右两部分，递归进行合并。

其结构与二叉树的后续遍历类似，即先处理左右子节点，再根据左右子节点的结果处理当前节点。归并的过程就是合并左右两个有序数组的过程。



```java
public class MergeSort {

    /**
     * 将数组转为全局变量，避免传参
     */
    private int[] arr;

    /**
     * 临时数组
     */
    private int[] temp;

    private void sort(int[] array) {
        arr = array;
        temp = new int[arr.length];
        mergeSort(0, array.length - 1);
        System.out.println(Arrays.toString(arr));
    }

    private void mergeSort(int left, int right) {
        if (left >= right) {
            return;
        }

        // 切记：采用移位运算符的话，要用括号括住
        int mid = left + ((right - left) >> 1);

        mergeSort(left, mid);
        mergeSort(mid + 1, right);
        merge(left, mid, right);
    }

    /**
     * 合并
     *
     * @param left
     * @param mid
     * @param right
     */
    private void merge(int left, int mid, int right) {
        // 左半边和右半边指针
        int p = left;
        int q = mid + 1;
        // 将数组缓存到临时数组
        for (int i = 0; i < arr.length; i++) {
            temp[i] = arr[i];
        }

        for (int cur = left; cur <= right; cur++) {
            if (p > mid) {
                arr[cur] = temp[q];
                q++;
            } else if (q > right) {
                arr[cur] = temp[p];
                p++;
            } else if (temp[p] > temp[q]) {
                arr[cur] = temp[q];
                q++;
            } else {
                arr[cur] = temp[p];
                p++;
            }
            
        }

    }


}

```



