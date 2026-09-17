# 239. ⭐Sliding Window Maximum

Date: Feb 7
Minutes: 90
Review Recommendation: ✅ Good job
Status: Done
Tags: MonoQueue, Stack&Queue

<aside>
💡

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return *the max sliding window*.

</aside>

Timer: ***1h+*** FAILD to find a possible solutoin

# Thought:

- For each window, get max and append to resoult list - **O(n*k)**
    - Prune:
        - If *preMax* popped out, recalculate max - **O(n)**;
        - else max is max(max, in) - **O(1)**
    - Exceed time limit.
- The key to reduce time complexity:
    - Maintain a **possible candidate**(second largest value) of window max when *preMax* is popped out.
    - Thus, we only need to compare the *candidate* and *in* - **O(1)**
- How to define a possible candidate for next step?
    - Only itmes **after** current max and **smaller than** current max
    - Buy induction, we need to form a monotonically decreasing container. And by using such a container, we can directly get the max from it.(*abandon the previous comparig solution*)
    - Consider the mechanism of slicing window, queue is appropriate.
- How to maintain such a **monotonically decreasing queue**?
    - Current max is at front most of the queue (so we can get max with **O(1)** time)
    - When pushing new itme(from slicing window), pop element in the rear, **until** the **element in front is no smaller than** new item.
    
    ```java
    while (!que.isEmpty() && n > que.peekLast()) 
    // must not write ">=", i.e. no dedup, 
    // because each number represents a positoin, 
    // dedup would cause window items to be popped out.
    // required for maintain mono queue after pop operation. 
    ```
    
    - When queue front(max) is out of slicing window, pop it out
- Evaluation: Need to fully understand monotonically queue!

> “大家貌似对单调队列 都有一些疑惑，首先要明确的是，题解中单调队列里的pop和push接口，仅适用于本题哈。单调队列不是一成不变的，而是不同场景不同写法，总之要保证队列里单调递减或递增的原则，所以叫做单调队列。 不要以为本题中的单调队列实现就是固定的写法哈。
> 
> 
> 大家貌似对deque也有一些疑惑，C++中deque是stack和queue默认的底层实现容器（这个我们之前已经讲过啦），deque是可以两边扩展的，而且deque里元素并不是严格的连续分布的。”
> 

# Solution

```java
class MonoQueue {
    private Deque<Integer> que = new ArrayDeque<>();

    public void poll(int n) {
        if (n == que.peekFirst()) {
            que.pollFirst();
        }
    }

    public void offer(int n) {
        while (!que.isEmpty() && n > que.peekLast()) { // no >= !
            que.pollLast();
        }

        que.offerLast(n);
    }

    public int peek() {
        return que.peekFirst();
    }
}

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        MonoQueue que = new MonoQueue();
    
        int n = nums.length;
        int[] result = new int[n - k + 1];

        for (int i = 0; i < k; i++) {
            que.offer(nums[i]);
        }

        result[0] = que.peek();

        for (int i = k; i < n; i++) { 
            que.poll(nums[i - k]);
            que.offer(nums[i]);

            result[i - k + 1] = que.peek();
        }

        return result;
    }
}
```