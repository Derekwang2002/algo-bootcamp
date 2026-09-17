# 225. Implement Stack using Queues

Date: Feb 6
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: Stack&Queue

<aside>
💡

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:

- `void push(int x)` Pushes element x to the top of the stack.
- `int pop()` Removes the element on the top of the stack and returns it.
- `int top()` Returns the element on the top of the stack.
- `boolean empty()` Returns `true` if the stack is empty, `false` otherwise.
</aside>

# Though:

- Use `queue1` to maintain regular order
- Use another `queue2` to temporarilly store ***n-1*** elements, get the last one in `queue1` for `pop()` and `top()`, then recover regular order in `queue1`
- `top()` need to deal with the remainning elements in `queue1`
- Evaluation:
    - Another approach is to main a LIFO order when pushing elements.
    - An optimal way is to use one single queue

# Solution

```java
class MyStack {
    Queue<Integer> que1;
    Queue<Integer> que2;

    public MyStack() {
        que1 = new ArrayDeque<>();
        que2 = new ArrayDeque<>();
    }
    
    public void push(int x) {
        que1.offer(x);
    }
    
    public int pop() {
        return touchLast(false);
    }
    
    public int top() {
        return touchLast(true);
    }
    
    public boolean empty() {
        return que1.isEmpty() && que2.isEmpty();
    }

    private int touchLast(boolean remain) {
        while (que1.size() != 1) { que2.offer(que1.poll()); }
        int res = que1.poll();
        if(remain) que2.offer(res);
        while (!que2.isEmpty()) { que1.offer(que2.poll()); }

        return res;
    }
}
```

## Maintain que1 as stack

```java
class MyStack {
    //q1作为主要的队列，其元素排列顺序和出栈顺序相同
    Queue<Integer> q1 = new ArrayDeque<>();
    //q2仅作为临时放置
    Queue<Integer> q2 = new ArrayDeque<>();

    public MyStack() {

    }
    //在加入元素时先将q1中的元素依次出栈压入q2，然后将新加入的元素压入q1，再将q2中的元素依次出栈压入q1
    public void push(int x) {
        while (q1.size() > 0) {
            q2.add(q1.poll());
        }
        q1.add(x);
        while (q2.size() > 0) {
            q1.add(q2.poll());
        }
    }

    public int pop() {
        return q1.poll();
    }

    public int top() {
        return q1.peek();
    }

    public boolean empty() {
        return q1.isEmpty();
    }
}
```

这个代码中的注释需要修改，可以提交pr

## One queue approach

```java
class MyStack {
    Queue<Integer> queue;
    
    public MyStack() {
        queue = new LinkedList<>();
    }
    
    public void push(int x) {
        queue.add(x);
    }
    
    public int pop() {
        rePosition();
        return queue.poll();
    }
    
    public int top() {
        rePosition();
        int result = queue.poll();
        queue.add(result);
        return result;
    }
    
    public boolean empty() {
        return queue.isEmpty();
    }

    public void rePosition(){
        int size = queue.size();
        size--;
        while(size-->0)
            queue.add(queue.poll());
    }
}
```