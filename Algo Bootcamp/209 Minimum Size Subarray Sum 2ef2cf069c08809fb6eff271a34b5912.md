# 209. Minimum Size Subarray Sum

Date: Jan 20
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: Array

<aside>
💡

Given an array of positive integers `nums` and a positive integer `target`, return *the **minimal length** of a subarray whose sum is greater than or equal to* `target`. If there is no such subarray, return `0` instead.

</aside>

# **思路**

- `subarray` 向右探索，记录所有满足条件的`subarray`中的最小长度 (双指针/滑动窗口)
    - 核心条件判断：`while left ≤ right` 当前`subarray`的和，
    `≥ target`时 比较最小长度 并抛弃左侧；
    `≤ target`时 添加右侧，当新的右指针`≥ len(nums)`时 break
- 评价：
    - 最直观的想法，动态维护subarray，每次循环动一步
    - **可读性和安全性稍差（`while True` + break）**
- 时间复杂度：$O(2n)$
- Carl 思路：
    - 优化点：
        - `for` 循环递增尾指针，内 `while` 判断并循环递减头指针
        - 最小长度用`min()`函数维护，比`if` 简洁

# Solution

## Mine

```python
class Solution(object):
    def minSubArrayLen(self, target, nums):
        """
        :type target: int
        :type nums: List[int]
        :rtype: int
        """
        
        # if len(nums) == 1:
        #     if nums[0] == target: return 1
        #     else: return 0

        n = len(nums)
        min_len = n + 1
        left, right = 0, 0 # double pointer
        sub_sum = nums[0]

        while True:
            sub_len = right - left + 1
            
            if sub_sum >= target:
                if left == right:
                    return 1
                if sub_len < min_len:
                    min_len = sub_len
                    
                sub_sum -= nums[left]
                left += 1
                
            else: # sub_sum < target               
                right += 1
                
                if right >= n:
                    break
                    
                sub_sum += nums[right]

        if min_len > n:
            return 0

        return min_len

```

# Carl’s

```python
class Solution:
    def minSubArrayLen(self, s: int, nums: List[int]) -> int:
        l = len(nums)
        left = 0
        right = 0
        min_len = float('inf')
        cur_sum = 0 #当前的累加值
        
        while right < l:
            cur_sum += nums[right]
            
            while cur_sum >= s: # 当前累加值大于目标值
                min_len = min(min_len, right - left + 1)
                cur_sum -= nums[left]
                left += 1
            
            right += 1
        
        return min_len if min_len != float('inf') else 0
```

```java
class Solution {

    // 滑动窗口
    public int minSubArrayLen(int s, int[] nums) {
        int left = 0;
        int sum = 0;
        int result = Integer.MAX_VALUE;
        for (int right = 0; right < nums.length; right++) {
            sum += nums[right];
            while (sum >= s) {
                result = Math.min(result, right - left + 1);
                sum -= nums[left++];
            }
        }
        return result == Integer.MAX_VALUE ? 0 : result;
    }
}
```