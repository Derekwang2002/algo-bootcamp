# 123. Best Time to Buy and Sell Stock III

Date: Mar 12
Level: Hard
Minutes: 60
Review Recommendation: 🚩 Recommend Redo (Hard >50m)
Status: Review-logic
Tags: DynamicProgram
Time Complexity: O(n)
Space Complexity: O(1)
URL: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/
Carl's: https://programmercarl.com/0123.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAIII.html

<aside>
💡

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete **at most two transactions**.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

</aside>

# Thought

> **FAILED!**
> 
- Four possible states(two transactions has order):
    - (Hidden state: no operation, omitted)
    - Hold stock 1st time(buy1)
    - Sold stock 1st time(sell1)
    - Hold stock 2nd time(buy2)
    - Sold stock 2nd time(sell2)
- Evaluation:
    - Hard to come up the states(key is order of transactions)
    - Initialization thought(at day 0): finish 2 trans in one day.

# Solution

```java
class Solution {
    public int maxProfit(int[] prices) {
        int buy1 = -prices[0];
        int sell1 = 0;
        int buy2 = -prices[0];
        int sell2 = 0;

        for (int i = 1; i < prices.length; i++) {
            buy1 = Math.max(buy1, -prices[i]);
            sell1 = Math.max(sell1, buy1 + prices[i]);
            buy2 = Math.max(buy2, sell1 - prices[i]);
            sell2 = Math.max(sell2, buy2 + prices[i]);
        }

        return sell2;
    }
}
```