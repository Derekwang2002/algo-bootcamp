# 279. Perfect Squares

Date: Mar 11
Level: Medium
Minutes: 15
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram, Knapsack
Time Complexity: O(mn)
Space Complexity: O(m)
URL: http://leetcode.com/problems/perfect-squares/description/
Carl's: https://programmercarl.com/0279.%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0.html

<aside>
💡

Given an integer `n`, return *the least number of perfect square numbers that sum to* `n`.

A **perfect square** is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, `1`, `4`, `9`, and `16` are perfect squares while `3` and `11` are not.

</aside>

# Thought

- Basically same as: [322. Coin Change](322%20Coin%20Change%203202cf069c0880bb9766e1e28918e0d1.md)
    - knapsack size → n
    - item: $(1^2, …, i^2)$ such that $i^2 ≤ n$

# Solution

```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, n + 1);
        dp[0] = 0;

        for (int i = 1; i * i <= n; i++) {
            for (int j = i * i; j <= n; j++) {
                dp[j] = Math.min(dp[j], dp[j - i * i] + 1);
            }
        }

        return dp[n];
    }
}
```