# 115. Distinct Subsequences

Date: Mar 19
Level: Hard
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram, String
Time Complexity: O(mn)
Space Complexity: O(n)
URL: https://leetcode.com/problems/distinct-subsequences/description/
Carl's: https://programmercarl.com/0115.%E4%B8%8D%E5%90%8C%E7%9A%84%E5%AD%90%E5%BA%8F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given two strings s and t, return *the number of distinct* ***subsequences** of* s *which equals* t.

The test cases are generated so that the answer fits on a 32-bit signed integer.

</aside>

# Thought

- **DP state:** dp(i,j) → number of distinct subsequences in s[0,j] equals t[0,i] (t.len ≤ s.len)
    - in practice, use t[0,i-1] & s[0,j-1] for convenience
- **DP equation:**
    - dp(i,j) = dp(i-1,j-1) + dp(i,j-1), if t[i] == s[j]
        - *dp(i-1,j-1)* → #subsequences(for t[0,i-1] & s[0,j-1]) could be concated with current characteristic.
        - *dp(i,j-1)* → #subsequences for the previous same characteristic
    - dp(i,j) = dp(i,j-1), if t[i] ≠ s[j]
- **Base case:**
    - dp(0,j) = 1
        - means only one subsequence of any string could be “”.
    - dp(i,0) = 0;
- Table:

```
dp[i][j] = # of ways to form t[0..i-1] from s[0..j-1]
 
    +---+---+---+---+---+---+---+---+
		| 0 | r | a | b | b | b | i | t |
+---+---+---+---+---+---+---+---+---+
| 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
+---+---+---+---+---+---+---+---+---+
| r | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
+---+---+---+---+---+---+---+---+---+
| a | 0 | 0 | 1 | 1 | 1 | 1 | 1 | 1 |
+---+---+---+---+---+---+---+---+---+
| b | 0 | 0 | 0 | 1 | 2 | 3 | 3 | 3 |
+---+---+---+---+---+---+---+---+---+
| b | 0 | 0 | 0 | 0 | 1 | 3 | 3 | 3 |
+---+---+---+---+---+---+---+---+---+
| i | 0 | 0 | 0 | 0 | 0 | 0 | 3 | 3 |
+---+---+---+---+---+---+---+---+---+
| t | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 3 |
+---+---+---+---+---+---+---+---+---+
```

# Solution

```java
class Solution {
    public int numDistinct(String s, String t) {
        int lenT = t.length();
        int[] dp = new int[lenT + 1];
        dp[0] = 1;

        for (char sc : s.toCharArray()) {
            for (int i = lenT; i >= 1; i--) {
                if (sc == t.charAt(i - 1)) dp[i] = dp[i - 1] + dp[i];
            }
        }

        return dp[lenT];
    }
}
```

![image.png](115%20Distinct%20Subsequences/f8143c0d-dcae-41eb-9b40-0f9411d096eb.png)