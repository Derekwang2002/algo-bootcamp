# 63. Unique Paths II

Date: Mar 7
Level: Medium
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram
Time Complexity: O(mn)
Space Complexity: O(n)
URL: https://leetcode.com/problems/unique-paths-ii/description/
Carl's: https://programmercarl.com/0063.%E4%B8%8D%E5%90%8C%E8%B7%AF%E5%BE%84II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

You are given an `m x n` integer array `grid`. There is a robot initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

An obstacle and space are marked as `1` or `0` respectively in `grid`. A path that the robot takes cannot include **any** square that is an obstacle.

Return *the number of possible unique paths that the robot can take to reach the bottom-right corner*.

The testcases are generated so that the answer will be less than or equal to `2 * 109`.

</aside>

# Thought

- Based on: [62. Unique Paths](62%20Unique%20Paths%203192cf069c0880739486ebd2a4b42d61.md)
- More: deal with obstacle
    - **If obstacle is in base row/column**, all palce after(including obstacle) along the row/column is zero
    - **If obstacle is in DP process**, skip it (default zero)

# Solution

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;

        // 1 row memoize
        int[] memo = new int[n];

        // initialize
        for (int i = 0; i < n; i++) {
            if (obstacleGrid[0][i] == 1) break;
            memo[i] = 1;
        }

        for (int r = 1; r < m; r++) {
            int[] row = new int[n];
            if (obstacleGrid[r][0] != 1) row[0] = memo[0]; // transferable

            for (int i = 1; i < n; i++) {
                if (obstacleGrid[r][i] == 1) continue;
                row[i] = row[i - 1] + memo[i];
            }
            
            memo = row;
        }

        return memo[n - 1];
    }
}
```