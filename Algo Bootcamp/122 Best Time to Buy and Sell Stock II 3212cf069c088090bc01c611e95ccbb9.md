# 122. Best Time to Buy and Sell Stock II

Date: Mar 12
Level: Medium
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram, Greedy
Time Complexity: O(n)
Space Complexity: O(1)
URL: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/
Carl's: https://programmercarl.com/0122.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAII%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can sell and buy the stock multiple times on the **same day**, ensuring you never hold more than one share of the stock.

Find and return *the **maximum** profit you can achieve*.

</aside>

# Thought

- Based on: [121. Best Time to Buy and Sell Stock](121%20Best%20Time%20to%20Buy%20and%20Sell%20Stock%203212cf069c088008a8d4f30cb898cb27.md)
- *One modification*, when held stock(at the end of the day), for the case that didn’t hold yesterday:
    - -prices[1] → dp[i-1][0] - prices[i]
    - Because one can have **cash gained before**(since can buy&sell multiple times), or to say, didn’t hold yesterday doesn’t means no cash.

# Solution

```java
class Solution {
    public int maxProfit(int[] prices) {
        int[] dp = new int[2];
        dp[1] = -prices[0];

        for (int i = 1; i < prices.length; i++) {
            int held = Math.max(dp[1], dp[0] - prices[i]); // hold stock
            int notHeld = Math.max(dp[0], dp[1] + prices[i]); // not hold

            dp[0] = notHeld;
            dp[1] = held;
        }

        return dp[0]; // must no stock held at last
    }
}
```

### Greedy

```java
class Solution {
    public int maxProfit(int[] prices) {
        int max = 0;
        int cur = prices[0];

        for (int p : prices) {
            if (p > cur) {
                max += p - cur;
            }

            cur = p;
        }

        return max;
    }
}
```