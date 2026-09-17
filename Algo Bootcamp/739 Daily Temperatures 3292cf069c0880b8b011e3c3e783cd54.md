# 739. Daily Temperatures

Date: Mar 20
Level: Medium
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: MonotonicStack
Time Complexity: O(n)
Space Complexity: O(n)
URL: https://leetcode.com/problems/daily-temperatures/
Carl's: https://programmercarl.com/0739.%E6%AF%8F%E6%97%A5%E6%B8%A9%E5%BA%A6.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given an array of integers `temperatures` represents the daily temperatures, return *an array* `answer` *such that* `answer[i]` *is the number of days you have to wait after the* `ith` *day to get a warmer temperature*. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

</aside>

# Thought

> 1-dimension, left/right-first-bigger/smaller item →**Monotonic stack**
> 
- Find right first bigger item:
    - direction: right-to-left push in stack
    - monolith: decrease from bottom to top(stack)
- Answer record:
    - stack item: **index** of temperatures
    - when an item is popped, record its distance from current index.

# Solution

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int[] ans = new int[temperatures.length];
        Deque<Integer> mono = new ArrayDeque<>();

        for (int i = 0; i < temperatures.length; i++) {
            while (!mono.isEmpty() && temperatures[i] > temperatures[mono.peek()]) {
                int index = mono.pop();
                ans[index] = i - index;
            }

            mono.push(i);
        }
        
        return ans;
    }
}
```