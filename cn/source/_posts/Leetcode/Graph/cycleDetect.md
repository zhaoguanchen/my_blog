---
title: 有向图的环检测
date: 
updated:
tags: Graph
categories: 
  - Leetcode
  - Graph
keywords: Graph
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

# 有向图的环检测

 

## 典型题解

### 1. Course Schedule 课程表

这道题是[Leetcode 207. Course Schedule](https://leetcode.com/problems/course-schedule/)。

#### 题目描述

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

 

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
```

**Example 2:**

```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```

 题目不难理解，什么时候无法修完所有课程？当存在循环依赖的时候。

其实这种场景在现实生活中也十分常见，比如我们写代码 import 包也是一个例子，必须合理设计代码目录结构，否则会出现循环依赖，编译器会报错，所以编译器实际上也使用了类似算法来判断你的代码是否能够成功编译。

**看到依赖问题，首先想到的就是把问题转化成「有向图」这种数据结构，只要图中存在环，那就说明存在循环依赖**。

具体来说，我们首先可以把课程看成「有向图」中的节点，节点编号分别是 `0, 1, ..., numCourses-1`，把课程之间的依赖关系看做节点之间的有向边。

比如说必须修完课程 `1` 才能去修课程 `3`，那么就有一条有向边从节点 `1` 指向 `3`。

[![img](https://labuladong.github.io/algo/images/%e6%8b%93%e6%89%91%e6%8e%92%e5%ba%8f/1.jpeg)](https://labuladong.github.io/algo/images/拓扑排序/1.jpeg)

如果发现这幅有向图中存在环，那就说明课程之间存在循环依赖，肯定没办法全部上完；反之，如果没有环，那么肯定能上完全部课程。



#### 思路分析

有向图最常见的存储方式是临接表，graph[i] 存放着i所指向的节点。除此之外，还有邻接矩阵。

```java
List<Integer>[] graph;
```

 本题可以采用BFS算法或DFS算法解决。

#### 题解：DFS

参考回溯算法，沿着依赖关系path进行递归，如发现遇到重复节点，则证明图中有环。

- 构建临接表，存储某节点的所有依赖关系
- 参考回溯算法进行DFS遍历，将节点添加到path，递归返回时撤销选择。
- 记录已访问过的节点，防止重复计算



```java
/**
 * 环检测算法 DFS版本
 * 利用临接表
 * 类比回溯算法
 */
class CourseScheduleDFS {

    /**
     * 默认会初始化为false，因此无需赋值
     */
    private boolean[] visited;

    private boolean[] path;

    private boolean hasCycle;

    private List<Integer>[] graph;

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        path = new boolean[numCourses];
        visited = new boolean[numCourses];
        graph = buildGraph(numCourses, prerequisites);
        System.out.println(Arrays.toString(graph));
        for (int i = 0; i < numCourses; i++) {
            traverse(i);
        }

        return !hasCycle;
    }


    private void traverse(int courseNumber) {
        // 历史路径中包含当前课程，出现环
        if (path[courseNumber]) {
            hasCycle = true;
            return;
        }

        // 已经遍历过且还未出现环，顺序一定要在判断环后面
        if (visited[courseNumber]) {
            return;
        }

        // 已经出现环
        if (hasCycle) {
            return;
        }


        visited[courseNumber] = true;
        path[courseNumber] = true;
        for (Integer nextCourseNum : graph[courseNumber]) {
            traverse(nextCourseNum);
        }
        path[courseNumber] = false;
    }

    private List<Integer>[] buildGraph(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        for (int i = 0; i < numCourses; i++) {
            graph[i] = new ArrayList<>();
        }

        for (int[] current : prerequisites) {
            int from = current[0];
            int to = current[1];
            graph[from].add(to);
        }
        return graph;
    }

}
```

#### 题解：BFS

BFS解法借助了图论中度的概念。所谓「出度」和「入度」是「有向图」中的概念，很直观：如果一个节点 `x` 有 `a` 条边指向别的节点，同时被 `b` 条边所指，则称节点 `x` 的出度为 `a`，入度为 `b`。

思路分析如下：

[![img](https://labuladong.github.io/algo/images/%e6%8b%93%e6%89%91%e6%8e%92%e5%ba%8f/5.jpeg)](https://labuladong.github.io/algo/images/拓扑排序/5.jpeg)

队列进行初始化后，入度为 0 的节点首先被加入队列：

[![img](https://labuladong.github.io/algo/images/%e6%8b%93%e6%89%91%e6%8e%92%e5%ba%8f/6.jpeg)](https://labuladong.github.io/algo/images/拓扑排序/6.jpeg)

开始执行 BFS 循环，从队列中弹出一个节点，减少相邻节点的入度，同时将新产生的入度为 0 的节点加入队列：

[![img](https://labuladong.github.io/algo/images/%e6%8b%93%e6%89%91%e6%8e%92%e5%ba%8f/7.jpeg)](https://labuladong.github.io/algo/images/拓扑排序/7.jpeg)

继续从队列弹出节点，并减少相邻节点的入度，这一次没有新产生的入度为 0 的节点：

[![img](https://labuladong.github.io/algo/images/%e6%8b%93%e6%89%91%e6%8e%92%e5%ba%8f/8.jpeg)](https://labuladong.github.io/algo/images/拓扑排序/8.jpeg)

继续从队列弹出节点，并减少相邻节点的入度，同时将新产生的入度为 0 的节点加入队列：

[![img](https://labuladong.github.io/algo/images/%e6%8b%93%e6%89%91%e6%8e%92%e5%ba%8f/9.jpeg)](https://labuladong.github.io/algo/images/拓扑排序/9.jpeg)

继续弹出节点，直到队列为空：

[![img](https://labuladong.github.io/algo/images/%e6%8b%93%e6%89%91%e6%8e%92%e5%ba%8f/10.jpeg)](https://labuladong.github.io/algo/images/拓扑排序/10.jpeg)

这时候，所有节点都被遍历过一遍，也就说明图中不存在环。

反过来说，如果按照上述逻辑执行 BFS 算法，存在节点没有被遍历，则说明成环。

比如下面这种情况，队列中最初只有一个入度为 0 的节点：

[![img](https://labuladong.github.io/algo/images/%e6%8b%93%e6%89%91%e6%8e%92%e5%ba%8f/11.jpeg)](https://labuladong.github.io/algo/images/拓扑排序/11.jpeg)

当弹出这个节点并减小相邻节点的入度之后队列为空，但并没有产生新的入度为 0 的节点加入队列，所以 BFS 算法终止：

[![img](https://labuladong.github.io/algo/images/%e6%8b%93%e6%89%91%e6%8e%92%e5%ba%8f/12.jpeg)](https://labuladong.github.io/algo/images/拓扑排序/12.jpeg)



算法分为以下5步：

**1、构建邻接表，和之前一样，边的方向表示「被依赖」关系。**

**2、构建一个 `indegree` 数组记录每个节点的入度，即 `indegree[i]` 记录节点 `i` 的入度。**

**3、对 BFS 队列进行初始化，将入度为 0 的节点首先装入队列。**

**4、开始执行 BFS 循环，不断弹出队列中的节点，减少相邻节点的入度，并将入度变为 0 的节点加入队列。**

**5、如果最终所有节点都被遍历过（`count` 等于节点数），则说明不存在环，反之则说明存在环。**



```java

/**
 * 环检测算法 BFS版本
 * 利用图论中度的性质
 */
class CourseScheduleBFS {


    public boolean canFinish(int numCourses, int[][] prerequisites) {
        int[] inDegree = new int[numCourses];
        List<Integer>[] graph = buildGraph(numCourses, prerequisites);
        // 入度表示被依赖  题中前者依赖后者，因此取edge[1]
        for (int[] edge : prerequisites) {
            int to = edge[1];
            inDegree[to]++;
        }

        int count = 0;
        Queue<Integer> queue = new LinkedList<>();
        // 加入所有入度为0的点（没有前置依赖）
        for (int i = 0; i < inDegree.length; i++) {
            if (0 == inDegree[i]) {
                queue.add(i);
            }
        }

        while (!queue.isEmpty()) {
            int num = queue.poll();
            count++;
            // 将它指向的节点的入度减一
            for (Integer item : graph[num]) {
                inDegree[item]--;
                // 已经无依赖，添加到队列
                if (0 == inDegree[item]) {
                    queue.add(item);
                }
            }
        }

        // 所有节点都被遍历到了，说明没有环
        return count == numCourses;
    }


    private List<Integer>[] buildGraph(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        for (int i = 0; i < numCourses; i++) {
            graph[i] = new ArrayList<>();
        }

        for (int[] current : prerequisites) {
            int from = current[0];
            int to = current[1];
            graph[from].add(to);
        }
        return graph;
    }
}
```



