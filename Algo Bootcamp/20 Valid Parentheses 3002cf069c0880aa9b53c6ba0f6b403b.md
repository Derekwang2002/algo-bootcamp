# 20. Valid Parentheses

Date: Feb 6
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: Stack&Queue

<aside>
💡

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.
</aside>

# Thought:

- Since there is no cross match, we can do it by single stack
- Loop s, if c is open brackets, push to stack; if c is close bracket, check if there is matching open one(isEmpty and isMatch).
- Evaluation:
    - Very classic one

# Solution

```java
class Solution {
    public boolean isValid(String s) {
        Deque<Character> stack =new ArrayDeque<>();

        for (Character c : s.toCharArray()) {
            if (c == '(' || c == '[' || c == '{') stack.push(c);
            if (c == ')' || c == ']' || c == '}') {
                if (
                    stack.isEmpty()
                    || !isMatch(stack.pop(), c)
                ) return false; //prune
            }
        }
        
        return stack.isEmpty();
    }

    private boolean isMatch(char open, char close) {
        if (open == '(') return close == ')';
        if (open == '[') return close == ']';
        if (open == '{') return close == '}';
        return false;
    }
}
```

## Cleaner

```java
class Solution {
    public boolean isValid(String s) {
        Deque<Character> deque = new LinkedList<>();
        char ch;
        for (int i = 0; i < s.length(); i++) {
            ch = s.charAt(i);
            //碰到左括号，就把相应的右括号入栈
            if (ch == '(') {
                deque.push(')');
            } else if (ch == '{') {
                deque.push('}');
            } else if (ch == '[') {
                deque.push(']');
            } else if (deque.isEmpty() || deque.peek() != ch) {
                return false; //如果是右括号判断是否和栈顶元素匹配
            }else { //右括号匹配
                deque.pop();
            }
        }
        //遍历结束，如果栈为空，则括号全部匹配
        return deque.isEmpty();
    }
}
```