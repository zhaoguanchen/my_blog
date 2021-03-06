---
title: 数据结构篇 -- LRU与LFU
date: 
updated:
tags: 
  - LRU
  - LFU
categories: 
  - Leetcode
  - 数据结构
  - LRU与LFU
keywords: 
  - LRU
  - LFU
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

 

# LRU

LRU 缓存淘汰算法就是一种常用策略。LRU 的全称是 Least Recently Used，也就是说我们认为最近使用过的数据应该是是「有用的」，很久都没用过的数据应该是无用的，内存满了就优先删那些很久没用过的数据。

要让 `put` 和 `get` 方法的时间复杂度为 O(1)，我们可以总结出 `cache` 这个数据结构必要的条件：

1、显然 `cache` 中的元素必须有时序，以区分最近使用的和久未使用的数据，当容量满了之后要删除最久未使用的那个元素腾位置。

2、我们要在 `cache` 中快速找某个 `key` 是否已存在并得到对应的 `val`；

3、每次访问 `cache` 中的某个 `key`，需要将这个元素变为最近使用的，也就是说 `cache` 要支持在任意位置快速插入和删除元素。

那么，什么数据结构同时符合上述条件呢？哈希表查找快，但是数据无固定顺序；链表有顺序之分，插入删除快，但是查找慢。所以结合一下，形成一种新的数据结构：哈希链表 `LinkedHashMap`。

LRU 缓存算法的核心数据结构就是哈希链表，双向链表和哈希表的结合体。

哈希表用来存取元素，双向链表用来控制使用顺序。**靠尾部的数据是最近使用的，靠头部的数据是最久为使用的**。



## 典型题解

### 1. LRU Cache

这道题是leetcode 146. LRU Cache.

LRU 缓存淘汰算法就是一种常用策略。LRU 的全称是 Least Recently Used，也就是说我们认为最近使用过的数据应该是是「有用的」，很久都没用过的数据应该是无用的，内存满了就优先删那些很久没用过的数据。

#### 题目描述

Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
- `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

 

**Example 1:**

```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

 

#### 思路分析

LRU 算法的核心数据结构是使用哈希链表 `LinkedHashMap`，首先借助链表的有序性使得链表元素维持插入顺序，同时借助哈希映射的快速访问能力使得我们可以在 O(1) 时间访问链表的任意元素。



**数据结构**

- 建立node，作为双向链表节点

- 维护一个HashMap和双向链表

**GET**

Get()方法执行时，若map中不存在元素，则返回-1；若存在，更新该元素在链表中的位置（即设为最近使用），并返回元素。

**PUT**

流程如下图所示。

![](https://imageguanchen.s3.us-east-2.amazonaws.com/leetcode/LRU-put.png) 



**设计**

- 多依靠面向对象思想，对各部功能进行多重封装。

#### 题解

```java
class Solution {
  
/**
 * 146. LRU Cache
 */
public class LRUCache {

    /**
     * 哈希表，可以O(1)复杂度找到节点
     */
    private Map<Integer, Node> hashMap;

    /**
     * 双向链表
     */
    private VList vList;

    /**
     * 容量
     */
    private int cap;

    /**
     * 私有方法
     * 在HashMap中根据key获取节点
     *
     * @param key
     * @return
     */
    private Node getNode(int key) {
        if (hashMap.containsKey(key)) {
            return hashMap.get(key);
        }
        return null;
    }

    /**
     * 更新节点
     * <p>
     * 即把节点移到链表末尾（先删除，后添加，时间复杂度都是O(1)）
     *
     * @param node
     */
    private void makeNew(Node node) {
        vList.delete(node);
        vList.add(node);
    }

    /**
     * 删除最老未使用元素
     */
    private void deleteOldest() {
        int key = vList.deleteLeft();
        hashMap.remove(key);
    }

    /**
     * 添加新元素
     */
    private void add(int key, int val) {
        int size = vList.getSize();
        // 容量已达上限
        if (size == cap) {
            deleteOldest();
        }
        Node node = new Node(key, val);
        hashMap.put(key, node);
        vList.add(node);
    }

    /**
     * 更新元素
     * <p>
     * 已存在key，则只需要更新val, 然后将节点放在链表末尾
     *
     * @param key
     * @param val
     */
    private void update(int key, int val) {
        Node node = hashMap.get(key);
        node.val = val;
        makeNew(node);
    }

    /**
     * 构造函数
     *
     * @param capacity
     */
    public LRUCache(int capacity) {
        vList = new VList();
        hashMap = new HashMap<>();
        cap = capacity;
    }

    /**
     * 获取元素值
     *
     * @param key
     * @return
     */
    public int get(int key) {
        Node node = getNode(key);
        if (node == null) {
            return -1;
        }

        makeNew(node);
        return node.val;
    }

    /**
     * 添加/更新元素值
     *
     * @param key
     * @param value
     */
    public void put(int key, int value) {
        if (hashMap.containsKey(key)) {
            update(key, value);
        } else {
            add(key, value);
        }
    }
}

/**
 * 节点定义
 * 包含key,val以及前后指针
 */
class Node {
    public int key;
    public int val;
    public Node prev;
    public Node next;

    public Node() {

    }

    public Node(int k, int v) {
        this.key = k;
        this.val = v;
    }
}

/**
 * 双向链表定义
 */
class VList {
    /**
     * 头尾节点
     */
    private Node head;

    private Node tail;
    /**
     * 链表实际大小
     */
    private int size;

    public VList() {
        this.size = 0;
        this.head = new Node();
        this.tail = new Node();
        head.next = tail;
        tail.prev = head;
    }

    /**
     * 获取链表大小
     *
     * @return
     */
    public int getSize() {
        return this.size;
    }

    /**
     * 插入新节点尾部
     *
     * @param node
     */
    public void add(Node node) {
        node.prev = tail.prev;
        node.prev.next = node;
        node.next = tail;
        tail.prev = node;
        size++;
    }

    /**
     * 删除节点（只需改变指针指向）
     *
     * @param node
     */
    public void delete(Node node) {
        node.next.prev = node.prev;
        node.prev.next = node.next;
        size--;
    }

    /**
     * 删除最左边元素，即为最久未访问元素
     * 删除节点，同时返回该节点的key，便于在HashMap中删除元素
     *
     * @return
     */
    public int deleteLeft() {
        Node oldest = head.next;
        delete(oldest);
        return oldest.key;
    }


}

 


```

 

### 2. LFU Cache

这道题是leetcode 146. LRU Cache。

LRU 算法的淘汰策略是 Least Recently Used，也就是每次淘汰那些最久没被使用的数据；而 LFU 算法的淘汰策略是 Least Frequently Used，也就是每次淘汰那些使用次数最少的数据。



#### 题目描述

Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
- `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

 

**Example 1:**

```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

 

#### 思路分析

**数据结构**

- 建立node，作为双向链表节点

- 维护一个HashMap和双向链表

**GET**

Get()方法执行时，若map中不存在元素，则返回-1；若存在，更新该元素在链表中的位置（即设为最近使用），并返回元素。

**PUT**

流程如下图所示。

![](https://imageguanchen.s3.us-east-2.amazonaws.com/leetcode/LRU-put.png) 



**设计**

- 多依靠面向对象思想，对各部功能进行多重封装。

#### 题解

```java
class Solution {
  
/**
 * 146. LRU Cache
 */
public class LRUCache {

    /**
     * 哈希表，可以O(1)复杂度找到节点
     */
    private Map<Integer, Node> hashMap;

    /**
     * 双向链表
     */
    private VList vList;

    /**
     * 容量
     */
    private int cap;

    /**
     * 私有方法
     * 在HashMap中根据key获取节点
     *
     * @param key
     * @return
     */
    private Node getNode(int key) {
        if (hashMap.containsKey(key)) {
            return hashMap.get(key);
        }
        return null;
    }

    /**
     * 更新节点
     * <p>
     * 即把节点移到链表末尾（先删除，后添加，时间复杂度都是O(1)）
     *
     * @param node
     */
    private void makeNew(Node node) {
        vList.delete(node);
        vList.add(node);
    }

    /**
     * 删除最老未使用元素
     */
    private void deleteOldest() {
        int key = vList.deleteLeft();
        hashMap.remove(key);
    }

    /**
     * 添加新元素
     */
    private void add(int key, int val) {
        int size = vList.getSize();
        // 容量已达上限
        if (size == cap) {
            deleteOldest();
        }
        Node node = new Node(key, val);
        hashMap.put(key, node);
        vList.add(node);
    }

    /**
     * 更新元素
     * <p>
     * 已存在key，则只需要更新val, 然后将节点放在链表末尾
     *
     * @param key
     * @param val
     */
    private void update(int key, int val) {
        Node node = hashMap.get(key);
        node.val = val;
        makeNew(node);
    }

    /**
     * 构造函数
     *
     * @param capacity
     */
    public LRUCache(int capacity) {
        vList = new VList();
        hashMap = new HashMap<>();
        cap = capacity;
    }

    /**
     * 获取元素值
     *
     * @param key
     * @return
     */
    public int get(int key) {
        Node node = getNode(key);
        if (node == null) {
            return -1;
        }

        makeNew(node);
        return node.val;
    }

    /**
     * 添加/更新元素值
     *
     * @param key
     * @param value
     */
    public void put(int key, int value) {
        if (hashMap.containsKey(key)) {
            update(key, value);
        } else {
            add(key, value);
        }
    }
}

/**
 * 节点定义
 * 包含key,val以及前后指针
 */
class Node {
    public int key;
    public int val;
    public Node prev;
    public Node next;

    public Node() {

    }

    public Node(int k, int v) {
        this.key = k;
        this.val = v;
    }
}

/**
 * 双向链表定义
 */
class VList {
    /**
     * 头尾节点
     */
    private Node head;

    private Node tail;
    /**
     * 链表实际大小
     */
    private int size;

    public VList() {
        this.size = 0;
        this.head = new Node();
        this.tail = new Node();
        head.next = tail;
        tail.prev = head;
    }

    /**
     * 获取链表大小
     *
     * @return
     */
    public int getSize() {
        return this.size;
    }

    /**
     * 插入新节点尾部
     *
     * @param node
     */
    public void add(Node node) {
        node.prev = tail.prev;
        node.prev.next = node;
        node.next = tail;
        tail.prev = node;
        size++;
    }

    /**
     * 删除节点（只需改变指针指向）
     *
     * @param node
     */
    public void delete(Node node) {
        node.next.prev = node.prev;
        node.prev.next = node.next;
        size--;
    }

    /**
     * 删除最左边元素，即为最久未访问元素
     * 删除节点，同时返回该节点的key，便于在HashMap中删除元素
     *
     * @return
     */
    public int deleteLeft() {
        Node oldest = head.next;
        delete(oldest);
        return oldest.key;
    }


}

 


```

 
