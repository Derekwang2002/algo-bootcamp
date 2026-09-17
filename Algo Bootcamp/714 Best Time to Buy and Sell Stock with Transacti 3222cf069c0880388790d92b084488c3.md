# 714. Best Time to Buy and Sell Stock with Transaction Fee

Date: Mar 13
Level: Medium
Minutes: 5
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram
Time Complexity: O(n)
Space Complexity: O(1)
URL: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/
Carl's: https://programmercarl.com/0714.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BA%E5%90%AB%E6%89%8B%E7%BB%AD%E8%B4%B9%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89.html

<aside>
💡

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `fee` representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

**Note:**

- You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
- The transaction fee is only charged once for each stock purchase and sale.
</aside>

# Thought

- Based on:[122. Best Time to Buy and Sell Stock II](122%20Best%20Time%20to%20Buy%20and%20Sell%20Stock%20II%203212cf069c088090bc01c611e95ccbb9.md)
    - add a transaction fee when selling the stock.

# Solution

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int[] dp = new int[2];
        dp[1] = -prices[0];

        for (int i = 1; i < prices.length; i++) {
            dp[0] = Math.max(dp[0], dp[1] + prices[i] - fee);
            dp[1] = Math.max(dp[1], dp[0] - prices[i]);
        }

        return dp[0];
    }
}
```