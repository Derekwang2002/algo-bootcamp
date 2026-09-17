# 232. Implement Queue using Stacks

Date: Feb 6
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: Stack&Queue

<aside>
💡

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:

- `void push(int x)` Pushes element x to the back of the queue.
- `int pop()` Removes the element from the front of the queue and returns it.
- `int peek()` Returns the element at the front of the queue.
- `boolean empty()` Returns `true` if the queue is empty, `false` otherwise.
</aside>

# Thought:

- Use ArrayDeque in Java to implment stack operations
- Implemenintg queue by pushing back and forth between two stacks
- Evaluation: Basic stack usage.
    - Use in/out stack is a better way!
    - ***always reuse code!!***

# Solution

```java
import java.util.Deque;
import java.util.ArrayDeque;

class MyQueue {
    Deque<Integer> stack1;
    Deque<Integer> stack2;

    public MyQueue() {
        stack1 = new ArrayDeque<>();
        stack2 = new ArrayDeque<>();
    }
    
    public void push(int x) {
        while (!stack2.isEmpty()) { stack1.push(stack2.pop()); }
        stack1.push(x);
    }
    
    public int pop() {
        while (!stack1.isEmpty()) { stack2.push(stack1.pop()); }
        return stack2.pop();
    }
    
    public int peek() {
        while (!stack1.isEmpty()) { stack2.push(stack1.pop()); }
        return stack2.peek();
    }
    
    public boolean empty() {
        return stack1.isEmpty() && stack2.isEmpty();
    }
}
```

## A better one with in/out stack

```java
class MyQueue {

    Stack<Integer> stackIn;
    Stack<Integer> stackOut;

    /** Initialize your data structure here. */
    public MyQueue() {
        stackIn = new Stack<>(); // 负责进栈
        stackOut = new Stack<>(); // 负责出栈
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        stackIn.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {    
        dumpstackIn();
        return stackOut.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        dumpstackIn();
        return stackOut.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stackIn.isEmpty() && stackOut.isEmpty();
    }

    // 如果stackOut为空，那么将stackIn中的元素全部放到stackOut中
    private void dumpstackIn(){
        if (!stackOut.isEmpty()) return; 
        while (!stackIn.isEmpty()){
                stackOut.push(stackIn.pop());
        }
    }
}
```