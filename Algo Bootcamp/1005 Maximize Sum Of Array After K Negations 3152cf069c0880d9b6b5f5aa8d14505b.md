# 1005. Maximize Sum Of Array After K Negations

Date: Feb 27
Level: Easy
Minutes: 30
Review Recommendation: 🚩 Recommend Redo (Easy >20m)
Status: Review-logic
Tags: Greedy
Time Complexity: O(nlogn)
Space Complexity: O(1)
URL: https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/description/
Carl's: https://programmercarl.com/1005.K%E6%AC%A1%E5%8F%96%E5%8F%8D%E5%90%8E%E6%9C%80%E5%A4%A7%E5%8C%96%E7%9A%84%E6%95%B0%E7%BB%84%E5%92%8C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given an integer array `nums` and an integer `k`, modify the array in the following way:

- choose an index `i` and replace `nums[i]` with `nums[i]`.

You should apply this process exactly `k` times. You may choose the same index `i` multiple times.

Return *the largest possible sum of the array after modifying it in this way*.

</aside>

# Thought

> Keep clear of different cases, thinl in greedy way!!
> 
- **Flipng nature**: two flips offset, so any k > 1 can convert into (0/1)
- **Greedy** strategy (presort nums):
    1. try to convert negaive number to postitive, 
    2. if no negative number, minize convertion:
        - convert smallest non-negative one.
- **Cases**(based on greedy strategy):
    - k ≤ #negatives
    - k ≥ #negatives
- **Implementation**:
    - loop, if k still left, convert negatives in nums if it still has
    
    ```java
    i < nums.length && k > 0
    ```
    
    - after that, two cases:
        - k is ran out → (k ≤ #negatives) (done)
        - k left → (k ≥ #negatives), means we need to flip non-negative numbers
            - left k is odd → smallest one
            - left is even → no flip need

# Solution

```java
class Solution {
    public int largestSumAfterKNegations(int[] nums, int k) {
        Arrays.sort(nums);

        int sum = 0;

        for (int i = 0; i < nums.length; i++) {
            if (k > 0) {
                if (nums[i] < 0) {
                    nums[i] *= -1;
                    k--;
                } else { // if nums has non-negative num, ends here
                    if (k % 2 > 0) { // must negete one (smallest)
                        if (i > 0 && nums[i - 1] < nums[i]) {
                            sum -= nums[i - 1] * 2;
                        } else {
                            nums[i] *= -1;
                        }
                    }
                    // running out of k
                    k = 0; 
                }
            }
        
            sum += nums[i];
        }

        // only when all nums < 0 and k > nums.length
        if (k > 0) {
            if (k % 2 > 0) {
                // math trick
                sum -= nums[nums.length - 1] * 2;
            }
        }

        return sum;
    }
}
```

## Carl’s (cleaner)

```java
class Solution {
    public int largestSumAfterKNegations(int[] nums, int k) {
        if (nums.length == 1) return nums[0];

        // 排序：先把负数处理了
        Arrays.sort(nums);

        for (int i = 0; i < nums.length && k > 0; i++) { // 通过负转正, 消耗尽可能多的k
            if (nums[i] < 0) {
                nums[i] = -nums[i];
                k--;
            }
        }

        // k < 0 -> k消耗完了不用讨论
        // k > 0 -> there are (only) non-nagative number need to be flipped
        
        // k > 0 && k is odd (if k is even，two flips offest)
        if (k % 2 == 1) { 
            Arrays.sort(nums); // 再次排序得到最小的正数
            nums[0] = -nums[0];
        }

        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        
        return sum;
    }
}
```