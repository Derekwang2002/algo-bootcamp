# 474. Ones and Zeroes

Date: Mar 10
Level: Medium
Minutes: 40
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram, Knapsack
Time Complexity: O(kmn )
Space Complexity: O(mn)
URL: https://leetcode.com/problems/ones-and-zeroes/
Carl's: https://programmercarl.com/0474.%E4%B8%80%E5%92%8C%E9%9B%B6.html

<aside>
💡

You are given an array of binary strings `strs` and two integers `m` and `n`.

Return *the size of the largest subset of `strs` such that there are **at most*** `m` **`0`*'s and* `n` **`1`*'s in the subset*.

A set `x` is a **subset** of a set `y` if all elements of `x` are also elements of `y`.

</aside>

# Thought

> 3-D DP table!
> 
- DP state(for s): dp[i][j]=maximum number of strings we can pick using at most i zeros and j ones (with strs[0] to s)
- DP equation(for s): $dp[i][j]=max(dp[i][j], 1+dp[i−zero][j−one])$
    - i≥zero,j≥one
        - zero=# of zeros in s
        - one=# of ones in s
    - Two cases:
        - **do not take** this string: dp[i][j]
        - **take** this string: 1+dp[i−zero][j−one]
- Base case: dp[i][j]=0
    - for all 0≤i≤m, 0≤j≤n0
- **DP table:**

```java
Step 0: Initialize

            0  1  2  3
          +--+--+--+--+
i=0       |0 |0 |0 |0 |
          +--+--+--+--+
i=1       |0 |0 |0 |0 |
          +--+--+--+--+
i=2       |0 |0 |0 |0 |
          +--+--+--+--+
i=3       |0 |0 |0 |0 |
          +--+--+--+--+
i=4       |0 |0 |0 |0 |
          +--+--+--+--+
i=5       |0 |0 |0 |0 |
          +--+--+--+--+

Step 1: "10" = (1,1)

            0  1  2  3
          +--+--+--+--+
i=0       |0 |0 |0 |0 |
          +--+--+--+--+
i=1       |0 |1 |1 |1 |
          +--+--+--+--+
i=2       |0 |1 |1 |1 |
          +--+--+--+--+
i=3       |0 |1 |1 |1 |
          +--+--+--+--+
i=4       |0 |1 |1 |1 |
          +--+--+--+--+
i=5       |0 |1 |1 |1 |
          +--+--+--+--+

Step 2: "0001" = (3,1)

            0  1  2  3
          +--+--+--+--+
i=0       |0 |0 |0 |0 |
          +--+--+--+--+
i=1       |0 |1 |1 |1 |
          +--+--+--+--+
i=2       |0 |1 |1 |1 |
          +--+--+--+--+
i=3       |0 |1 |1 |1 |
          +--+--+--+--+
i=4       |0 |1 |2 |2 |
          +--+--+--+--+
i=5       |0 |1 |2 |2 |
          +--+--+--+--+

Step 3: "111001" = (2,4)   -> no change

            0  1  2  3
          +--+--+--+--+
i=0       |0 |0 |0 |0 |
          +--+--+--+--+
i=1       |0 |1 |1 |1 |
          +--+--+--+--+
i=2       |0 |1 |1 |1 |
          +--+--+--+--+
i=3       |0 |1 |1 |1 |
          +--+--+--+--+
i=4       |0 |1 |2 |2 |
          +--+--+--+--+
i=5       |0 |1 |2 |2 |
          +--+--+--+--+

Step 4: "1" = (0,1)

            0  1  2  3
          +--+--+--+--+
i=0       |0 |1 |1 |1 |
          +--+--+--+--+
i=1       |0 |1 |2 |2 |
          +--+--+--+--+
i=2       |0 |1 |2 |2 |
          +--+--+--+--+
i=3       |0 |1 |2 |2 |
          +--+--+--+--+
i=4       |0 |1 |2 |3 |
          +--+--+--+--+
i=5       |0 |1 |2 |3 |
          +--+--+--+--+

Step 5: "0" = (1,0)

            0  1  2  3
          +--+--+--+--+
i=0       |0 |1 |1 |1 |
          +--+--+--+--+
i=1       |1 |2 |2 |2 |
          +--+--+--+--+
i=2       |1 |2 |3 |3 |
          +--+--+--+--+
i=3       |1 |2 |3 |3 |
          +--+--+--+--+
i=4       |1 |2 |3 |3 |
          +--+--+--+--+
i=5       |1 |2 |3 |4 |
          +--+--+--+--+
```

# Solution

```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m + 1][n + 1];

        for (String s : strs) {
            int zero = countZero(s);
            int one = s.length() - zero;

            for (int i = m; i >= zero; i--) {
                for (int j = n; j >= one; j--) {
                    dp[i][j] = Math.max(dp[i][j], 1 + dp[i - zero][j - one]);
                }
            }
        }

        return dp[m][n];
    }

    int countZero(String s) {
        int count = 0;
        for (char c : s.toCharArray()) {
            if (c == '0') count++;
        }        
        return count;
    }
}
```