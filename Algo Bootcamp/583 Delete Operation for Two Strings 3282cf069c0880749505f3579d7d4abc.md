# 583. Delete Operation for Two Strings

Date: Mar 19
Level: Medium
Minutes: 5
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram, String
Time Complexity: O(mn)
Space Complexity: O(mn)
URL: https://leetcode.com/problems/delete-operation-for-two-strings/
Carl's: https://programmercarl.com/0583.%E4%B8%A4%E4%B8%AA%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%9A%84%E5%88%A0%E9%99%A4%E6%93%8D%E4%BD%9C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given two strings `word1` and `word2`, return *the minimum number of **steps** required to make* `word1` *and* `word2` *the same*.

In one **step**, you can delete exactly one character in either string.

</aside>

# Thought

### Method 1

- DP state:
    - **dp(i,j) → min steps to make word1[0, i], word2[0,j] same**
    - in practice, use *word1[0, i-1], word2[0,j-1]* for convenience
- DP equation:
    - dp(i,j) = dp(i-1,j-1), if word1[i] == word2[j]
    - dp(i,j) = min{dp(i-1,j), dp(i,j-1)} + 1, if if word1[i] ≠ word2[j]
        - dp(i-1,j) + 1 → delete word1[i]
        - dp(i,j-1) + 1 → delete word2[j]
- Base case:
    - dp(k,0) = k, k in [0,word1.length]
    - dp(0,k) = k, k in [0,word1.length]
- Table:

```
            ""   e   a   t
          +----+---+---+---+
      ""  |  0 | 1 | 2 | 3 |
          +----+---+---+---+
      s   |  1 | 2 | 3 | 4 |
          +----+---+---+---+
      e   |  2 | 1 | 2 | 3 |
          +----+---+---+---+
      a   |  3 | 2 | 1 | 2 |
          +----+---+---+---+
```

### Method 2

- Use [1143. Longest Common Subsequence](1143%20Longest%20Common%20Subsequence%203262cf069c0880ca918bf4554282df43.md) to get length of LCS
- Return (words1.length - LCS) + (words2.length - LCS)

# Solution

### Method 1

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int len1 = word1.length();
        int len2 = word2.length();
        int[][] dp = new int[len1 + 1][len2 + 1];

        for (int k = 1; k <= len1; k++) dp[k][0] = k;
        for (int k = 1; k <= len2; k++) dp[0][k] = k;

        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + 1;
                }
            }
        }

        return dp[len1][len2];
    }
}
```

### Method 2

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int len1 = word1.length();
        int len2 = word2.length();
        int[][] dp = new int[len1 + 1][len2 + 1];

        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        return len1 + len2 - 2 * dp[len1][len2];
    }
}
```