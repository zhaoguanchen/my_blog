---
title: 前缀树算法
date: 
updated:
tags: 前缀树算法
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

# 前缀树算法

 





```java
public class Trie {

    /**
     * 虚拟根节点
     */
    private TrieNode root;

    /**
     * Trie的构造函数
     */
    public Trie() {
        root = new TrieNode();
    }

    public void insert(String word) {
        TrieNode curNode = root;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            // 不存在则新建节点
            if (!curNode.containsKey(c)) {
                curNode.put(c, new TrieNode());
            }
            // 继续向下层寻找
            curNode = curNode.get(c);
        }
        curNode.setEnd();
    }

    public boolean search(String word) {
        TrieNode curNode = root;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            // 不存在当前字符，直接返回false
            if (!curNode.containsKey(c)) {
                return false;
            }
            // 继续向下层寻找
            curNode = curNode.get(c);
        }
        return curNode.isEnd();
    }

    public boolean startsWith(String prefix) {
        TrieNode curNode = root;
        for (int i = 0; i < prefix.length(); i++) {
            char c = prefix.charAt(i);
            // 不存在则新建节点
            if (!curNode.containsKey(c)) {
                return false;
            }
            curNode = curNode.get(c);
        }
        // 只要prefix都存在，不管isEnd()，都可以返回true
        return true;
    }


}

class TrieNode {

    /**
     * 子节点
     */
    private TrieNode[] children;

    /**
     * 数组范围   当前仅考虑26个小写字母
     */
    private final int range = 26;

    /**
     * 是否为终止节点（到达单词末字符）
     */
    private boolean isEnd;

    /**
     * 构造函数
     */
    public TrieNode() {
        children = new TrieNode[range];
        isEnd = false;
    }

    public void setEnd() {
        isEnd = true;
    }

    public boolean isEnd() {
        return isEnd;
    }

    /**
     * 子节点是否包含字符c
     *
     * @param c
     * @return
     */
    public boolean containsKey(char c) {
        // 找到索引
        int index = c - 'a';
        return null != children[index];
    }

    /**
     * 查找对应字符所在的Node
     *
     * @param c
     * @return
     */
    public TrieNode get(char c) {
        int index = c - 'a';
        return children[index];
    }

    /**
     * 添加元素， 将对应索引位置的子节点赋值为新的TrieNode
     *
     * @param c
     * @param trieNode
     */
    public void put(char c, TrieNode trieNode) {
        int index = c - 'a';
        children[index] = trieNode;
    }
}
```

