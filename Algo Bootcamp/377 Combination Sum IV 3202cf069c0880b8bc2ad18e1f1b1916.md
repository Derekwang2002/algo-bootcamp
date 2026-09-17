# 377. Combination Sum IV

Date: Mar 10
Level: Medium
Minutes: 60
Review Recommendation: 🚩 Recommend Redo (Medium >40m)
Status: Review-logic
Tags: DynamicProgram, Knapsack
Time Complexity: O(mn)
Space Complexity: O(m)
URL: https://leetcode.com/problems/combination-sum-iv/description/
Carl's: https://programmercarl.com/0377.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C%E2%85%A3.html

<aside>
💡

Given an array of **distinct** integers `nums` and a target integer `target`, return *the number of possible combinations that add up to* `target`.

The test cases are generated so that the answer can fit in a **32-bit** integer.

</aside>

# Thought

- **DP state**: dp(i,j) → #sequences, usig front **i** numbers to sum up **j**
- DP equation: $dp(i,j) = \sum_{n_k\in nums} dp(i,j-n_k)$
    - $dp(i,j-n_k)$ **represents not using $n_k$ as last number.**
    - Only related to the former numbers in the same row(i)
    - ***Compressed***: for each sum j → `dp[j] += dp[j - n] for n in nums`
        - `d[j]` means #sequences *using all numbers* to sum up j
- Base case: dp(i,0) = 1 for any i

# Solution

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;

        for (int i = 0; i < nums.length; i++) {
            for (int j = nums[i]; j <= target; j++) {
                dp[j] = 0;

                for (int k = 0; k <= i; k++) {
                    if (j < nums[k]) continue;
                    dp[j] += dp[j - nums[k]];
                }
            }
        }

        return dp[target];
    }
}
```

### Optimized

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;

        for (int j = 1; j <= target; j++) {
            for (int n : nums) {
                if (n > j) continue;
                dp[j] += dp[j - n];
            }
        }

        return dp[target];
    }
}
```