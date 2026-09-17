# 516. Longest Palindromic Subsequence

Date: Mar 20
Level: Medium
Minutes: 60
Review Recommendation: 🚩 Recommend Redo (Medium >40m)
Status: Done
Tags: DynamicProgram
Time Complexity: O(n^2)
Space Complexity: O(n^2)
URL: https://leetcode.com/problems/longest-palindromic-subsequence/description/
Carl's: https://programmercarl.com/0516.%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E5%BA%8F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given a string `s`, find *the longest palindromic **subsequence**'s length in* `s`.

A **subsequence** is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

</aside>

# Thought

- DP state: longest palindromic subsequence in range s[i,j]
    - j ≥ i
- DP equation:
    - dp(i,j) = 1, if i=j
    - dp(i,j) = dp(i+1,j-1) + 2, if s[i]=s[j] && i≠j
    - dp(i,j) = max{dp(i+1,j), dp(i,j-1)}, if s[i]≠s[j]
- Table:

```java
				j=0(b)  j=1(b)  j=2(b)  j=3(a)  j=4(b)
      +-------+-------+-------+-------+-------+
i=0(b)|   1   |   2   |   3   |   3   |   4   | <--- Final Result: dp[0][4]
      +-------+-------+-------+-------+-------+
i=1(b)|       |   1   |   2   |   2   |   3   |
      +-------+-------+-------+-------+-------+
i=2(b)|       |       |   1   |   1   |   2   |
      +-------+-------+-------+-------+-------+
i=3(a)|       |       |       |   1   |   1   |
      +-------+-------+-------+-------+-------+
i=4(b)|       |       |       |       |   1   |
      +-------+-------+-------+-------+-------+
```

# Solution

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int len = s.length();
        int[][] dp = new int[len][len];
        
        for (int i = len - 1; i >= 0; i--) {
            for (int j = i; j < len; j++) {
            
                if (i == j) {
                    dp[i][j] = 1;
                    continue;
                }
                
                if (s.charAt(i) == s.charAt(j)) dp[i][j] = dp[i + 1][j - 1] + 2;
                else dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }

        return dp[0][len - 1];
    }
}
```

### Optimized

```java
public int longestPalindromeSubseq(String s) {
    int n = s.length();
    int[] dp = new int[n];

    for (int i = n - 1; i >= 0; i--) {
        dp[i] = 1; // Base case: diagonal dp[i][i] = 1
        int prev = 0; // This stores the old dp[i+1][j-1]
        
        for (int j = i + 1; j < n; j++) {
            int temp = dp[j]; // Save current dp[j] before overwriting
            
            if (s.charAt(i) == s.charAt(j)) {
                dp[j] = prev + 2;
            } else {
                dp[j] = Math.max(dp[j], dp[j - 1]);
            }
            prev = temp; // Update prev for the next j
        }
    }
    return dp[n - 1];
}
```