# 496. Next Greater Element I

Date: Mar 20
Level: Easy
Minutes: 60
Review Recommendation: 🚩 Recommend Redo (Easy >20m)
Status: Done
Tags: MonotonicStack
Time Complexity: O(m+n)
Space Complexity: O(n)
URL: https://leetcode.com/problems/next-greater-element-i/description/
Carl's: https://programmercarl.com/0496.%E4%B8%8B%E4%B8%80%E4%B8%AA%E6%9B%B4%E5%A4%A7%E5%85%83%E7%B4%A0I.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

The **next greater element** of some element `x` in an array is the **first greater** element that is **to the right** of `x` in the same array.

You are given two **distinct 0-indexed** integer arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

For each `0 <= i < nums1.length`, find the index `j` such that `nums1[i] == nums2[j]` and determine the **next greater element** of `nums2[j]` in `nums2`. If there is no next greater element, then the answer for this query is `-1`.

Return *an array* `ans` *of length* `nums1.length` *such that* `ans[i]` *is the **next greater element** as described above.*

</aside>

# Thought

> nums1/nums2 are item-unique → hashmap
> 
- Use monotonic stack to find first-greater number in nums2, store the result as (number, first-right-greater) in hashmap (when popped)
- The numbers left in the monotonic stack is no greater number in right, set those number as (number, -1) in hashmap
- Traverse nums1, put corresponding result into answer array.

# Solution

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int[] ans = new int[nums1.length];
        Deque<Integer> mono = new ArrayDeque<>();
        Map<Integer, Integer> map = new HashMap<>(); // key to save time

        for (int n : nums2) {
            while (!mono.isEmpty() && n > mono.peek()) map.put(mono.pop(), n);
            mono.push(n);
        }

        for (int n : mono) map.put(n, -1);
        for (int i = 0; i < nums1.length; i++) ans[i] = map.get(nums1[i]);

        return ans;
    }
}
```