# 53. Maximum Subarray

Date: Feb 26
Level: Medium
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram, Greedy
Time Complexity: O(n)
Space Complexity: O(1)
URL: https://leetcode.com/problems/maximum-subarray/description/
Carl's: https://programmercarl.com/0053.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given an integer array `nums`, find the subarray with the largest sum, and return *its sum*.

</aside>

# Thought

### Greedy

- Loop nums `for (int n : nums)` and sum
- When the `n` is bigger than current sum and current sum < 0, drop all past sum(negetive effect) and let curretn sum = `n`.
    - Condition: [`n` is bigger than current sum] is for the case that *n is negetive and smaller than current sum*
- Record max sum at each iteration.

# Solution

## Greedy

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = nums[0];
        int curSum = 0;

        for (int n : nums) {
            if (n > curSum && curSum < 0) {
                curSum = n; // renew - drop left part
            } else {
                curSum += n;
            }

            maxSum = Math.max(curSum , maxSum);
        }

        return maxSum;
    }
}
```

### Carl’s

```java
class Solution {
    public int maxSubArray(int[] nums) {
        if (nums.length == 1){
            return nums[0];
        }
        int sum = Integer.MIN_VALUE;
        int count = 0;
        for (int i = 0; i < nums.length; i++){
            count += nums[i];
            sum = Math.max(sum, count); // 取区间累计的最大值（相当于不断确定最大子序终止位置）
            if (count <= 0){
                count = 0; // 相当于重置最大子序起始位置，因为遇到负数一定是拉低总和
            }
        }
       return sum;
    }
}
```