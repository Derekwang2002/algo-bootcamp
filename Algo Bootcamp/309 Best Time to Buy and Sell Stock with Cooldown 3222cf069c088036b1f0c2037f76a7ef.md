# 309. Best Time to Buy and Sell Stock with Cooldown

Date: Mar 13
Level: Medium
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram
Time Complexity: O(n)
Space Complexity: O(1)
URL: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/
Carl's: https://programmercarl.com/0309.%E6%9C%80%E4%BD%B3%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E6%97%B6%E6%9C%BA%E5%90%AB%E5%86%B7%E5%86%BB%E6%9C%9F.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

- After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

</aside>

# Thought

- Based on:[122. Best Time to Buy and Sell Stock II](122%20Best%20Time%20to%20Buy%20and%20Sell%20Stock%20II%203212cf069c088090bc01c611e95ccbb9.md)
- Break ***not hold* state** → *selling* + *cooling*
    - selling(i) equation: dp[i-1, selling] + price
        - a seperate operation
    - cooling(i) equation: max{dp[i-1, cooling], dp[i-1, selling]}
        - case 1: cooling at yesterday
        - case 2: sold at yesterday

# Solution

```java
class Solution {
    public int maxProfit(int[] prices) {
        int[] dp = new int[3];
        dp[1] = -prices[0];

        for (int price : prices) {
            int sell = dp[1] + price;
            int buy = Math.max(dp[1], dp[0] - price);
            int cool = Math.max(dp[0], dp[2]);
            
            dp[0] = cool;
            dp[1] = buy;
            dp[2] = sell;
        }

        return Math.max(dp[0], dp[2]);
    }
}
```