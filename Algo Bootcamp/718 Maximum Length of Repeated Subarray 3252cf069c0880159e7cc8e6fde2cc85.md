# 718. Maximum Length of Repeated Subarray

Date: Mar 15
Level: Medium
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram
Time Complexity: O(mn)
Space Complexity: O(n)
URL: https://leetcode.com/problems/maximum-length-of-repeated-subarray
Carl's: https://programmercarl.com/0718.%E6%9C%80%E9%95%BF%E9%87%8D%E5%A4%8D%E5%AD%90%E6%95%B0%E7%BB%84.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given two integer arrays `nums1` and `nums2`, return *the maximum length of a subarray that appears in **both** arrays*.

</aside>

# Thought

- DP state: dp(i,j) → longest repeated subarray length end with nums1[i]/nums2[j]
- Base case: dp(0,0) = 0
- DP equation:
    - dp(i,j) = dp(i-1,j-1) + 1 if nums1[i] == nums2[j]
    - dp(i,j) = 0 if if nums1[i] ≠ nums2[j]
- Table:

```
                nums2 →
          +---+---+---+---+---+
          | 0 | 3 | 2 | 1 | 4 | 7 |
+---------+---+---+---+---+---+---+
| 0       | 0 | 0 | 0 | 0 | 0 | 0 |
+---------+---+---+---+---+---+---+
| 1       | 0 | 0 | 0 | 1 | 0 | 0 |
+---------+---+---+---+---+---+---+
| 2       | 0 | 0 | 1 | 0 | 0 | 0 |
+---------+---+---+---+---+---+---+
| 3       | 0 | 1 | 0 | 0 | 0 | 0 |
+---------+---+---+---+---+---+---+
| 2       | 0 | 0 | 2 | 0 | 0 | 0 |
+---------+---+---+---+---+---+---+
| 1       | 0 | 0 | 0 | 3 | 0 | 0 |
+---------+---+---+---+---+---+---+
```

# Solution

```java
class Solution {
    public int findLength(int[] nums1, int[] nums2) {
        int[] dp = new int[nums2.length + 1];
        dp[0] = 0;
        int ans = 0;

        for (int i = 0; i < nums1.length; i++) {
            for (int j = nums2.length; j > 0 ; j--) {
                if (nums1[i] == nums2[j - 1]) dp[j] = dp[j - 1] + 1;
                else dp[j] = 0;
                ans = Math.max(ans, dp[j]);
            }
        }

        return ans;
    }
}
```