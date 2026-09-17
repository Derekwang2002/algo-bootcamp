# 674. Longest Continuous Increasing Subsequence

Date: Mar 15
Level: Easy
Minutes: 5
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram
Time Complexity: O(n)
Space Complexity: O(1)
URL: https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/
Carl's: https://programmercarl.com/0674.%E6%9C%80%E9%95%BF%E8%BF%9E%E7%BB%AD%E9%80%92%E5%A2%9E%E5%BA%8F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given an unsorted array of integers `nums`, return *the length of the longest **continuous increasing subsequence** (i.e. subarray)*. The subsequence must be **strictly** increasing.

A **continuous increasing subsequence** is defined by two indices `l` and `r` (`l < r`) such that it is `[nums[l], nums[l + 1], ..., nums[r - 1], nums[r]]` and for each `l <= i < r`, `nums[i] < nums[i + 1]`.

</aside>

# Thought

- A simpler version of [300. Longest Increasing Subsequence](300%20Longest%20Increasing%20Subsequence%203252cf069c08809b94bafdfc8b087aa4.md)
- Difference → DP equation
    - Since the subarray is **continuous**, the algo doesn’t need to traversion prefix at each $i$, just compare with previous one.
        - dp[i] = dp[i-1] + 1 if nums[i] > nums[i-1]
        - dp[i] = 1 if nums[i] ≤ nums[i-1]
- Return the biggest dp value.

# Solution

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int dp = 1;
        int ans = 1;

        for (int i = 1; i < nums.length; i++) {
            if (nums[i] > nums[i - 1]) dp++;
            else dp = 1;
            ans = Math.max(ans, dp);
        }

        return ans;
    }
}
```