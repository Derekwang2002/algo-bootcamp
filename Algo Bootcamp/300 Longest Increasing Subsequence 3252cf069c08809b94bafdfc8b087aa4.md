# 300. Longest Increasing Subsequence

Date: Mar 15
Level: Medium
Minutes: 60
Review Recommendation: 🚩 Recommend Redo (Medium >40m)
Status: Done
Tags: DynamicProgram
Time Complexity: O(n^2)
Space Complexity: O(n)
URL: https://leetcode.com/problems/longest-increasing-subsequence/
Carl's: https://programmercarl.com/0300.%E6%9C%80%E9%95%BF%E4%B8%8A%E5%8D%87%E5%AD%90%E5%BA%8F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given an integer array `nums`, return *the length of the longest **strictly increasing subsequence***.

</aside>

# Thought

- DP state: dp[i]→the longest increasing subsequence end with nums[i]
- Base case: at least 1 for each item
- DP equation:
    
    ```java
    for (int j = 0; j < i; j++) {
        if (nums[j] < nums[i]) dp[i] = Math.max(dp[i], dp[j] + 1);
    }
    ```
    

# Solution

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        Arrays.fill(dp, 1);
        int ans = 1;

        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
		            // equation
                if (nums[j] < nums[i]) dp[i] = Math.max(dp[i], dp[j] + 1);
            }
            
            ans = Math.max(ans, dp[i]);
        }

        return ans;
    }
}
```

### Carl’s

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums.length <= 1) return nums.length;
        int[] dp = new int[nums.length];
        int res = 1;
        Arrays.fill(dp, 1);
        
        for (int i = 1; i < dp.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            res = Math.max(res, dp[i]);
        }
        
        return res;
    }
}
```