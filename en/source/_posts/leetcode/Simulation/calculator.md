---
title: How to implement a basic claculator?
date: 
updated:
tags: simulation
categories: 
  - Leetcode
  - simulation
keywords: claculator
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



How to implement a basic claculator?

The answer is using `Stack`.



# 1.  `'+'`, `'-'`, `'*'`, `'/'`

At first, we will only consider `'+', '-', '*', '/'` and some number of spaces. 

It is also the problem [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/). 



## Solution

 We use Deque for better Iteration and use Stack to store the result.

```java
class Solution {
  
    public int calculate(String s) {
        Deque<Character> queue = new LinkedList<>();

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
						// ignore the space
            if (' ' == c) {
                continue;
            }
            queue.add(c);
        }

        return helper(queue);
    }

    private int helper(Deque<Character> queue) {
        char sign = '+';
        Stack<Integer> stack = new Stack<>();
        int num = 0;

        while (!queue.isEmpty()) {
            char c = queue.pollFirst();
						
            // digit
            if (Character.isDigit(c)) {
                num = num * 10 + (c - '0');
            }

          	// remember to add queue.isEmpty(), since the last num should be involved.
            if (!Character.isDigit(c) || queue.isEmpty()) {
                if ('+' == sign) {
                    stack.add(num);
                }
                if ('-' == sign) {
                    stack.add(-num);
                }
                if ('*' == sign) {
                    int pre = stack.pop();
                    stack.push(pre * num);
                }
                if ('/' == sign) {
                    int pre = stack.pop();
                    stack.push(pre / num);
                }
								
              // reset num and sign
                num = 0;
                sign = c;
            }
            
        }
				
      	// calculate sum
        int sum = 0;
        while (!stack.isEmpty()) {
            sum += stack.pop();
        }

        return sum;
    }

}
```



# 2.  `'+'`, `'-'`, `'*'`, `'/'`, `'('`, and `')'`.

Then, we will only consider `'+'`, `'-'`, `'*'`, `'/'`, `'('`, and `')'`.

It is also the problem [772. Basic Calculator III](https://leetcode.com/problems/basic-calculator-iii/). 



## Solution

 We use Deque for better Iteration and use Stack to store the result.

When encounter `'('`,  start Recursion.

When encounter `')'`,  stop.



```java
class Solution {
    public int calculate(String s) {
        Deque<Character> queue = new LinkedList<>();

        for (int i = 0; i < s.length(); i++) {
            queue.add(s.charAt(i));
        }


        return helper(queue);
    }

    private int helper(Deque<Character> queue) {
        Stack<Integer> s = new Stack<>();
        int num = 0;
        char sign = '+';

        while (!queue.isEmpty()) {
            char c = queue.pollFirst();

            if (Character.isDigit(c)) {
                num = num * 10 + (c - '0');
            }

            if ('(' == c) {
                num = helper(queue);
            }
						// remember to add queue.isEmpty(), since the last num should be involved.
            if (!Character.isDigit(c) || queue.isEmpty()) {
                if ('+' == sign) {
                    s.add(num);
                }

                if ('-' == sign) {
                    s.add(-num);
                }

                if ('*' == sign) {
                    int pre = s.pop();
                    s.add(pre * num);
                }

                if ('/' == sign) {
                    int pre = s.pop();
                    s.add(pre / num);
                }
								
              // reset
                num = 0;
                sign = c;
            }

            if (')' == c) {
                break;
            }

        }

        int sum = 0;
        while (!s.isEmpty()) {
            sum += s.pop();
        }

        return sum;
    }
}
```

