# 53. Maximum Subarray

Date: Mar 17
Level: Medium
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram
Time Complexity: O(n)
Space Complexity: O(1)
URL: https://leetcode.com/problems/maximum-subarray/description/
Carl's: https://programmercarl.com/0053.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89.html

<aside>
💡

Given an integer array `nums`, find the subarray with the largest sum, and return *its sum*.

</aside>

# Thought

> Greedy version: [53. Maximum Subarray](53%20Maximum%20Subarray%203142cf069c0880ecbe11d651fd841d8b.md)
> 
- Dynamic Programming:
    - state: dp[i] → max-sum subarray end with nums[i]
    - equation: dp[i] = max{nums[i], dp[i-1] + nums[i]}
        - base case: dp[0] = nums[0];
    - result: record the max dp within the loop
- DP Table:

```
index   nums[i]   dp(以i结尾最大和)   ans(全局最大)
---------------------------------------------------
0       -2        -2                  -2
          初始化

1        1        max(1, -2+1=-1)=1    1
                   ↑ 重新开始

2       -3        max(-3, 1-3=-2)=-2   1
                   ↑ 接上更优

3        4        max(4, -2+4=2)=4     4
                   ↑ 重新开始 ⭐

4       -1        max(-1, 4-1=3)=3     4
                   ↑ 接上

5        2        max(2, 3+2=5)=5      5
                   ↑ 接上 ⭐

6        1        max(1, 5+1=6)=6      6 ⭐
                   ↑ 接上

7       -5        max(-5, 6-5=1)=1     6

8        4        max(4, 1+4=5)=5      6
```

# Solution

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int dp = nums[0];
        int ans = dp;

        for (int i = 1; i < nums.length; i++) {
            dp = Math.max(nums[i], dp + nums[i]);
            ans = Math.max(dp, ans);
        }

        return ans;
    }
}
```