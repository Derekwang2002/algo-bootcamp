# 213. House Robber II

Date: Mar 11
Level: Medium
Minutes: 50
Review Recommendation: 🚩 Recommend Redo (Medium >40m)
Status: Done
Tags: DynamicProgram
Time Complexity: O(n)
Space Complexity: O(1)
URL: https://leetcode.com/problems/house-robber-ii/description/
Carl's: https://programmercarl.com/0213.%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8DII.html

<aside>
💡

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight without alerting the police*.

</aside>

# Thought

- Based on: [198. House Robber](198%20House%20Robber%203212cf069c08804d9575f70701c113dd.md)
- Since first & last house is adjacent, they can’t be chose simultaneously
    - Get max dp result of {nums[0, last-1], nums[1,last]}
    
    ```java
    return Math.max(noLast, noFirst);
    ```
    

# Solution

```java
class Solution {
    public int rob(int[] nums) {
        int[] dp = new int[2];
        dp[1] = nums[0];
        if (nums.length == 1) return dp[1];

        for (int i = 1; i < nums.length - 1; i++) { // no last
            int cur = Math.max(dp[1], dp[0] + nums[i]);
            dp[0] = dp[1];
            dp[1] = cur;
        }

        int noLast = dp[1];

        dp[0] = 0;
        dp[1] = nums[1];

        for (int i = 2; i < nums.length; i++) { // no first
            int cur = Math.max(dp[1], dp[0] + nums[i]);
            dp[0] = dp[1];
            dp[1] = cur;
        }

        return Math.max(noLast, dp[1]);
    }
}
```