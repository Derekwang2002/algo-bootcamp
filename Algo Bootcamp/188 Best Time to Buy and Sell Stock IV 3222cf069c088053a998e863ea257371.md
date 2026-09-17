# 188. Best Time to Buy and Sell Stock IV

Date: Mar 12
Level: Hard
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram
Time Complexity: O(n)
Space Complexity: O(1)
URL: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/
Carl's: https://programmercarl.com/0647.%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `k`.

Find the maximum profit you can achieve. You may complete at most `k` transactions: i.e. you may buy at most `k` times and sell at most `k` times.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

</aside>

# Thought

- Similar to [123. Best Time to Buy and Sell Stock III](123%20Best%20Time%20to%20Buy%20and%20Sell%20Stock%20III%203212cf069c08801fa793d8b1617fbaf9.md)
    - An extension, expand 2 transactions to k transactions.

# Solution

```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        int[][]dp = new int[2][k+1];
        Arrays.fill(dp[0], -prices[0]); // hold

        for (int price : prices) {
            for (int i = 1; i <= k; i++) {
                dp[0][i] = Math.max(dp[1][i - 1] - price, dp[0][i]); // hold
                dp[1][i] = Math.max(dp[0][i] + price, dp[1][i]); // sold
            }
        }

        return dp[1][k];
    }
}
```