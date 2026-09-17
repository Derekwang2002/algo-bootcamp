# 518. Coin Change II

Date: Mar 10
Level: Medium
Minutes: 5
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram, Knapsack
Time Complexity: O(mn)
Space Complexity: O(m)
URL: https://leetcode.com/problems/coin-change-ii/description/
Carl's: https://programmercarl.com/0518.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the number of combinations that make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

</aside>

# Thought

- Full knapsack
- DP state: dp[i,j]=number of ways to make amount j using 1-ith coins.
- DP equation:
    - Normal: $dp[i][j]=dp[i-1][j]+dp[i][j-c[i]]$
        - dp[i-1][j] → not use ith coin
        - dp[i][j-c[i]] → use one ith coin
    - Compress:  $dp[j]=dp[j]+dp[j-c]$
        - Forward filling
- Base case: dp[0]=1
    - one way: pick nothing
- DP table:

```
amount →
        0 1 2 3 4 5
      ------------
init    1 0 0 0 0 0

coin 1  1 1 1 1 1 1

coin 2  1 1 2 2 3 3

coin 5  1 1 2 2 3 4
```

# Solution

```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;

        for (int c : coins) {
            for (int j = c; j <= amount; j++) {
                dp[j] += dp[j - c];
            }
        }

        return dp[amount];
    }
}
```