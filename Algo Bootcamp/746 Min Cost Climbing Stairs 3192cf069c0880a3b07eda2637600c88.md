# 746. Min Cost Climbing Stairs

Date: Mar 4
Level: Easy
Minutes: 10
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram
Time Complexity: O(n)
Space Complexity: O(1)
URL: https://leetcode.com/problems/min-cost-climbing-stairs
Carl's: https://programmercarl.com/0746.%E4%BD%BF%E7%94%A8%E6%9C%80%E5%B0%8F%E8%8A%B1%E8%B4%B9%E7%88%AC%E6%A5%BC%E6%A2%AF.html

<aside>
💡

You are given an integer array `cost` where `cost[i]` is the cost of `ith` step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index `0`, or the step with index `1`.

Return *the minimum cost to reach the top of the floor*.

</aside>

# Thought

- DP state: dp[i]=minimum cost to reach step i
- DP equation: $dp[i]=min(dp[i−1]+cost[i−1],dp[i−2]+cost[i−2])$
- Base case: dp[0]=0, dp[1]=0

# Solution

```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int n = cost.length;
        if (n < 2) return 0;

        int[] dp = new int[2];
        dp[0] = 0;
        dp[1] = 0;

        for (int i = 2; i <= n; i++) {
            int sum = Math.min(dp[0] + cost[i - 2], dp[1] + cost[i - 1]);
            dp[0] = dp[1];
            dp[1] = sum;
        }

        return dp[1];
    }
}
```