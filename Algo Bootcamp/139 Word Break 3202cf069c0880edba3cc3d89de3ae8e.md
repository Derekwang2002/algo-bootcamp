# 139. Word Break

Date: Mar 11
Level: Medium
Minutes: 30
Review Recommendation: ✅ Good job
Status: Review-Java
Tags: DynamicProgram, Knapsack, String
Time Complexity: O(mn)
Space Complexity: O(m)
URL: https://leetcode.com/problems/word-break/
Carl's: https://programmercarl.com/0139.%E5%8D%95%E8%AF%8D%E6%8B%86%E5%88%86.html

<aside>
💡

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

</aside>

# Thought

> **Sequence!**(substrings orders) + Full knapsack
> 
- **DP state**: $dp[j]$ → Whether the first **j** characters of string s can be formed by concatenating words from `wordDict`.
- DP equation:
    
    $$
    dp[j] = \cup_{w\in wordDict}\ \{dp[j - w.length()] \cap s.startsWith(w, j - w.length())\}
    
    $$
    
    - Column by column iteration
- Base case: dp[0] = true → pick nothing can form an empty string
- *Note*:
    - `str.startsWith(s, i)` return(boolean) if (exist subtring of `str` that start at `i`) equals `s`
    - *much faster than create a new subtring object!*

# Solution

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;

        for (int j = 1; j <= s.length(); j++) {
            for (String w : wordDict) {
                if (j < w.length()) continue;
                boolean cur = w.equals(s.substring(j - w.length(), j));
                dp[j] = dp[j] || (dp[j - w.length()] && cur);
            }
        }

        return dp[s.length()];
    }
}
```

### Better string method

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;

        for (int j = 1; j <= s.length(); j++) {
            for (String w : wordDict) {
                if (j < w.length()) continue;
                
                // use startsWith instead substring，avoid new string
                if (dp[j - w.length()] && s.startsWith(w, j - w.length())) {
                    dp[j] = true;
                    break; // found one valid, jump to next j
                }
            }
        }

        return dp[s.length()];
    }
}
```