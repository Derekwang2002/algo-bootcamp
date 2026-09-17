# 1049. Last Stone Weight II

Date: Mar 10
Level: Medium
Minutes: 5
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram, Knapsack
Time Complexity: O(mn)
Space Complexity: O(m)
URL: https://leetcode.com/problems/last-stone-weight-ii/
Carl's: https://programmercarl.com/1049.%E6%9C%80%E5%90%8E%E4%B8%80%E5%9D%97%E7%9F%B3%E5%A4%B4%E7%9A%84%E9%87%8D%E9%87%8FII.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

You are given an array of integers `stones` where `stones[i]` is the weight of the `ith` stone.

We are playing a game with the stones. On each turn, we choose any two stones and smash them together. Suppose the stones have weights `x` and `y` with `x <= y`. The result of this smash is:

- If `x == y`, both stones are destroyed, and
- If `x != y`, the stone of weight `x` is destroyed, and the stone of weight `y` has new weight `y - x`.

At the end of the game, there is **at most one** stone left.

Return *the smallest possible weight of the left stone*. If there are no stones left, return `0`.

</aside>

# Thought

- Pretty much same as: [416. Partition Equal Subset Sum](416%20Partition%20Equal%20Subset%20Sum%2031e2cf069c0880118b15db14e11e01f3.md)
- Trick: pick two sub piles of stones, the minimum difference of it is the answer.
    - So the algo should Divide the stones into two piles with  **weight as equal as possible**.
- DP state:
- Base case
- DP equatin:

# Solution

```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for (int s : stones) sum += s;
        int size = sum / 2;

        int[] dp = new int[size + 1];

        for (int n : stones) {
            for (int i = size; i >= n; i--) { // sack size bigger than n
                dp[i] = Math.max(dp[i], dp[i - n] + n);
            }
        }

        return sum - 2 * dp[size];
    }
}
```