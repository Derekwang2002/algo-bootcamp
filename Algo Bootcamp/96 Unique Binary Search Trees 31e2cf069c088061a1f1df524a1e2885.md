# 96. Unique Binary Search Trees

Date: Mar 8
Level: Medium
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram
Time Complexity: O(n^2)
Space Complexity: O(n)
URL: https://leetcode.com/problems/unique-binary-search-trees/description/
Carl's: https://programmercarl.com/0096.%E4%B8%8D%E5%90%8C%E7%9A%84%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given an integer `n`, return *the number of structurally unique **BST'**s (binary search trees) which has exactly* `n` *nodes of unique values from* `1` *to* `n`.

</aside>

# Thought

- DP state: dp[i]=number of unique BSTs formed using i nodes
- DP equation: $dp[i]=∑_{k=1}^idp[k−1]⋅dp[i−k]$
    - pick node k as root,
    - left subtree has k-1 nodes,
    - right subtree has i-k nodes.
- Base case: dp(1) = 1

# Solution

```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1];
        dp[1] = 1;

        for (int i = 2; i <= n; i++) {
            dp[i] = 2 * dp[i - 1];
            for (int j = 1; j < i - 1; j++) {
                dp[i] += dp[j] * dp[i - j - 1];
            }
        }

        return dp[n];
    }
}
```