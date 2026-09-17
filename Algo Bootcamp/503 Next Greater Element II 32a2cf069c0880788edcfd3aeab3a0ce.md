# 503. Next Greater Element II

Date: Mar 20
Level: Medium
Minutes: 50
Review Recommendation: 🚩 Recommend Redo (Medium >40m)
Status: Done
Tags: MonotonicStack
Time Complexity: O(n)
Space Complexity: O(n)
URL: https://leetcode.com/problems/next-greater-element-ii/description/
Carl's: https://programmercarl.com/0503.%E4%B8%8B%E4%B8%80%E4%B8%AA%E6%9B%B4%E5%A4%A7%E5%85%83%E7%B4%A0II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given a circular integer array `nums` (i.e., the next element of `nums[nums.length - 1]` is `nums[0]`), return *the **next greater number** for every element in* `nums`.

The **next greater number** of a number `x` is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return `-1` for this number.

</aside>

# Thought

- Based on: [739. Daily Temperatures](739%20Daily%20Temperatures%203292cf069c0880b8b011e3c3e783cd54.md)
- After filling answer array first time, there may be numbers left in monotonic stack, **which has no greater number in uncircular array**
    - *traverse `nums` again,* and compare with monotonic stack top.
    - *if greater*, record to answer array, and stay in the current number (because it may be the answer of the next number in monotonic stak)

# Solution

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        Deque<Integer> mono = new ArrayDeque<>();
        int[] ans = new int[nums.length];
        Arrays.fill(ans, -1);

        for (int i = 0; i < nums.length; i++) {
            while (!mono.isEmpty() && nums[i] > nums[mono.peek()]) {
                int index = mono.pop();
                ans[index] = nums[i];
            }
            mono.push(i);
        }

        for (int i = 0; i < nums.length; i++) {
            if (mono.isEmpty()) break;
            int index = mono.peek();
            
            if (nums[i] > nums[index]) {
                ans[index] = nums[i];
                mono.pop();
                i--;
            }
        }

        return ans;
    }
}
```