# 122. Best Time to Buy and Sell Stock II

Date: Feb 27
Level: Medium
Minutes: 5
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram, Greedy
Time Complexity: O(n)
Space Complexity: O(1)
URL: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/
Carl's: https://programmercarl.com/0376.%E6%91%86%E5%8A%A8%E5%BA%8F%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

<aside>
💡

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can sell and buy the stock multiple times on the **same day**, ensuring you never hold more than one share of the stock.

Find and return *the **maximum** profit you can achieve*.

</aside>

# Thought

- Record price of the day before as pre
- Compare today’s price and pre, if there is an increase, add the  margin into total max profit.

# Solution

```java
class Solution {
    public int maxProfit(int[] prices) {
        int max = 0;
        int pre = prices[0];

        for (int p : prices) {
            if (p > pre) {
                max += p - pre;
            }

            pre = p;
        }

        return max;
    }
}
```