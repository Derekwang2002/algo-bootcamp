# 150. Evaluate Reverse Polish Notation

Date: Feb 7
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: Stack&Queue

<aside>
💡

You are given an array of strings `tokens` that represents an arithmetic expression in a [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Evaluate the expression. Return *an integer that represents the value of the expression*

- **Note** that:
    - The valid operators are `'+'`, `'-'`, `'*'`, and `'/'`.
    - Each operand may be an integer or another expression.
    - The division between two integers always **truncates toward zero**.
    - There will not be any division by zero.
    - The input represents a valid arithmetic expression in a reverse polish notation.
    - The answer and all the intermediate calculations can be represented in a **32-bit** integer.
</aside>

# Thought:

- Loop s, if it is:
- a number, push to stack;
- an operator, pop two from stack, and push the calculation result
- return stack.peek() (int).
- Evaluation:
    - Classic one
    - Be care of the type transfer methods.

# Solution

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<String> stack = new ArrayDeque<>();

        for (String s : tokens) {
            if ("+-*/".contains(s)) {
                String opRight = stack.pop();
                String opLeft = stack.pop();
                int result = getResult(opLeft, opRight, s);
                stack.push(String.valueOf(result));
            } else {
                stack.push(s);
            }
        }

        return Integer.parseInt(stack.peek());
    }

    private int getResult(String l, String r, String op) {
        int lv = Integer.parseInt(l);
        int rv = Integer.parseInt(r);
        int res;
        if (op.equals("+")) {
            res = lv + rv;
        } else if (op.equals("-")) {
            res = lv - rv;
        } else if (op.equals("*")) {
            res = lv * rv;
        } else {
            res = lv / rv;
        }
        return res;
    }
}
```

## Less type transfer

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<Integer> stack = new LinkedList();
        for (String s : tokens) {
            if ("+".equals(s)) {     // leetcode 内置jdk的问题，不能使用==判断字符串是否相等
                stack.push(stack.pop() + stack.pop());      // 注意 - 和/ 需要特殊处理
            } else if ("-".equals(s)) {
                stack.push(-stack.pop() + stack.pop());
            } else if ("*".equals(s)) {
                stack.push(stack.pop() * stack.pop());
            } else if ("/".equals(s)) {
                int temp1 = stack.pop();
                int temp2 = stack.pop();
                stack.push(temp2 / temp1);
            } else {
                stack.push(Integer.valueOf(s));
            }
        }
        return stack.pop();
    }
}
```