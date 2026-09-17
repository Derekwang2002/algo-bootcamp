# 121. Best Time to Buy and Sell Stock

Date: Mar 12
Level: Easy
Minutes: 20
Review Recommendation: ✅ Good job
Status: Review-logic
Tags: DynamicProgram, Greedy
Time Complexity: O(n)
Space Complexity: O(1)
URL: https://leetcode.com/problems/best-time-to-buy-and-sell-stock/
Carl's: https://programmercarl.com/0121.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BA.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return *the maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`.

</aside>

# Thought

- **Analysis**:
    - for a single day, there are 2 cases, (hold / not hold) stock
    - holding the stock, 2 cases
        - case 1: held yesterday
        - case 2: didn’t hold yesterday, buy today
    - not holding the stock, 2 cases
        - case 1: didn’t hold yesterday
        - case 2: held yesterday, sell today
    - need a n x 2 size dp table
- **DP state:**
    - dp[i][0] → max cash when hold stock at day i
    - dp[i][1] → max cash when not hold stock at day i
- **DP equation:**
    - dp[i][0] = max\{dp[i-1][0], -prices[i]\}
    - dp[i][1] = max\{dp[i-1][1], prices[1] + dp[i-1][0]\}
    - *Base case:*
        - dp[0][0] = -prices[0];
        - dp[0][1] = 0;

# Solution

### Dynamic Programming

```java
class Solution {
    public int maxProfit(int[] prices) {
        int[] dp = new int[2];
        dp[0] = -prices[0];

        for (int i = 1; i < prices.length; i++) {
            dp[1] = Math.max(dp[1], prices[i] + dp[0]); // not hold stock
            dp[0] = Math.max(dp[0], -prices[i]); // hold stock
        }

        return dp[1];
    }
}
```

### Greedy

```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """

        min_price = prices[0]
        max_profit = 0

        for i in range(len(prices)):
            profit = prices[i] - min_price
            if profit > max_profit: max_profit = profit 
            if prices[i] < min_price: min_price = prices[i]
            

        return max_profit
```