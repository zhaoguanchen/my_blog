---
title: Traverse a Tree
date: 
updated:
tags: Tree
categories: 
  - Leetcode
  - Tree
keywords: Traverse
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

# Traverse a Tree

## 1. Level Order Traversal

Use BFS method.

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) {
            return new ArrayList<>();
        }
        
        List<List<Integer>> ans = new ArrayList<>();
       
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> list = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode cur = queue.poll();
                list.add(cur.val);
                if (cur.left != null) {
                    queue.add(cur.left);
                }
                if (cur.right != null) {
                    queue.add(cur.right);
                }
            }
            
            ans.add(list);  
        }
        
        return ans;       
    }
}
```





## 2. Preorder Traversal

### Recursion

```java
class Solution {
  
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        helper(root, ans);
        return ans;
    }


    private void helper(TreeNode root, List<Integer> ans) {
        if (root == null) {
            return;
        }
				ans.add(root.val);
        helper(root.left, ans);
        helper(root.right, ans);
    }

}
```

### Iteration

```java
class Solution {
  
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        stack.add(root);

        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            ans.add(node.val);

            if (node.right != null) {
                stack.add(node.right);
            }

            if (node.left != null) {
                stack.add(node.left);
            }
        }

        return ans;

    }
 

}
```



## 3. Inorder Traversal

### Recursion

```java
class Solution {
  
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        helper(root, ans);
        return result;
    }


    private void helper(TreeNode root, List<Integer> ans) {
        if (root == null) {
            return;
        }

        helper(root.left, ans);
        result.add(root.val);
        helper(root.right, ans);
    }

}
```

### Iteration

```java
class Solution {
  
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();

        TreeNode curr = root;
        while (curr != null || !stack.isEmpty()) {
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }
            curr = stack.pop();
            res.add(curr.val);
            curr = curr.right;
        }
      
        return res;

    }
 

}
```



## 4. Postorder Traversal

### Recursion

```java
class Solution {
  
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        helper(root, ans);
        return ans;
    }


    private void helper(TreeNode root, List<Integer> ans) {
        if (root == null) {
            return;
        }
				
        helper(root.left, ans);
        helper(root.right, ans);
        ans.add(root.val);
    }

}
```

### Iteration

```java
class Solution {
  
    public List<Integer> postorderTraversal(TreeNode root) {
        LinkedList<Integer> ans = new LinkedList<>();
        Stack<TreeNode> stack = new Stack<>();
        stack.add(root);

        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
          	// add first
            ans.addFirst(node.val);

            if (node.left != null) {
                stack.add(node.left);
            }
          
            if (node.right != null) {
                stack.add(node.right);
            }
        }

        return ans;

    }
 

}
```



## 5. Vertical Order Traversal

Use BFS method. Store Pair of node and column.

```java
class Solution {
    
    public List<List<Integer>> verticalOrder(TreeNode root) {
        Map<Integer, List<Integer>> map = new HashMap<>();
        Queue<Pair<TreeNode, Integer>> queue = new LinkedList<>();
        queue.add(new Pair<>(root, 0));

        while (!queue.isEmpty()) {
            Pair<TreeNode, Integer> pair = queue.poll();
            TreeNode node = pair.getKey();
            if (node == null) {
                continue;
            }
            int col = pair.getValue();
            if (!map.containsKey(col)) {
                map.put(col, new ArrayList<>());
            }

            map.get(col).add(node.val);
            queue.add(new Pair<>(node.left, col - 1));
            queue.add(new Pair<>(node.right, col + 1));

        }
        // sort by key
        List<Integer> list = new ArrayList<>(map.keySet());
        Collections.sort(list);
        // generate result
        List<List<Integer>> ans = new ArrayList<>();
        for (int i : list) {
            ans.add(map.get(i));
        }

        return ans;
    }
   
}
```

