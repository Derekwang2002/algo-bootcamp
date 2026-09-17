# 1143. Longest Common Subsequence

Date: Mar 16
Level: Medium
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram
Time Complexity: O(mn)
Space Complexity: O(mn)
URL: https://leetcode.com/problems/longest-common-subsequence/
Carl's: https://programmercarl.com/1143.%E6%9C%80%E9%95%BF%E5%85%AC%E5%85%B1%E5%AD%90%E5%BA%8F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given two strings `text1` and `text2`, return *the length of their longest **common subsequence**.* If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

</aside>

# Thought

- Uncontinuous version of: [718. Maximum Length of Repeated Subarray](718%20Maximum%20Length%20of%20Repeated%20Subarray%203252cf069c0880159e7cc8e6fde2cc85.md)
- DP state: **dp[i][j]**→The longest common subsequence of the substring `text1[0, i - 1]` and the substring `text2[0, j - 1].`
- Base case:
    - dp[0][j] = 0;
    - dp[i][0] = 0;
- DP equation:
    - if text1[i-1] == text2[j-1]:
        - dp[i][j] = dp[i-1][j-1] + 1
    - if text1[i-1] ≠ text2[j-1]:
        - dp[i][j] = max{dp[i-1][j], dp[i][j-1]}
- Table:

```
                0   a   c   e
              +---+---+---+---+
          0   | 0 | 0 | 0 | 0 |
              +---+---+---+---+
          a   | 0 |[1]| 1 | 1 |
              +---+---+---+---+
          b   | 0 | 1 | 1 | 1 |
              +---+---+---+---+
          c   | 0 | 1 |[2]| 2 |
              +---+---+---+---+
          d   | 0 | 1 | 2 | 2 |
              +---+---+---+---+
          e   | 0 | 1 | 2 |[3]|
              +---+---+---+---+

路径：
a → c → e
```

# Solution

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int len1 = text1.length();
        int len2 = text2.length();
        int[][] dp = new int[len1 + 1][len2 + 1];

        for (int i = 1; i <= len1; i++) {
            for (int j = 1; j <= len2; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        return dp[len1][len2];
    }
}
```

### Compressed

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n1 = text1.length();
        int n2 = text2.length();

        // 多从二维dp数组过程分析  
        // 关键在于  如果记录  dp[i - 1][j - 1]
        // 因为 dp[i - 1][j - 1]  <!=>  dp[j - 1]  <=>  dp[i][j - 1]
        int [] dp = new int[n2 + 1];

        for(int i = 1; i <= n1; i++){

            // 这里pre相当于 dp[i - 1][j - 1]
            int pre = dp[0];
            for(int j = 1; j <= n2; j++){

                //用于给pre赋值
                int cur = dp[j];
                if(text1.charAt(i - 1) == text2.charAt(j - 1)){
                    //这里pre相当于dp[i - 1][j - 1]   千万不能用dp[j - 1] !!
                    dp[j] = pre + 1;
                } else{
                    // dp[j]     相当于   dp[i - 1][j]
                    // dp[j - 1] 相当于   dp[i][j - 1]
                    dp[j] = Math.max(dp[j], dp[j - 1]);
                }

                //更新dp[i - 1][j - 1], 为下次使用做准备
                pre = cur;
            }
        }

        return dp[n2];
    }
}
```

- **store dp[j] before change** as real dp[i-1][j-1] for next usage