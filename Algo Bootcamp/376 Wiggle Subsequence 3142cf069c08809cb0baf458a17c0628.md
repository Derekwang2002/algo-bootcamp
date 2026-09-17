# 376. Wiggle Subsequence

Date: Feb 26
Level: Medium
Minutes: 45
Review Recommendation: 🚩 Recommend Redo (Medium >40m)
Status: Done
Tags: DynamicProgram, Greedy
Time Complexity: O(n)
Space Complexity: O(n)
URL: https://leetcode.com/problems/wiggle-subsequence/description/
Carl's: https://programmercarl.com/0376.%E6%91%86%E5%8A%A8%E5%BA%8F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

A **wiggle sequence** is a sequence where the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with one element and a sequence with two non-equal elements are trivially wiggle sequences.

- For example, `[1, 7, 4, 9, 2, 5]` is a **wiggle sequence** because the differences `(6, -3, 5, -7, 3)` alternate between positive and negative.
- In contrast, `[1, 4, 7, 2, 5]` and `[1, 7, 4, 5, 5]` are not wiggle sequences. The first is not because its first two differences are positive, and the second is not because its last difference is zero.

A **subsequence** is obtained by deleting some elements (possibly zero) from the original sequence, leaving the remaining elements in their original order.

Given an integer array `nums`, return *the length of the longest **wiggle subsequence** of* `nums`.

</aside>

# Thought

> A lot details / edge cases!
> 
1. Head & tail element
2. Plane between: 
    1. monotonic 
    2. wiggle

### Greedy #1 (compicate)

- First filt case nums.length is 1
- Get differences array `dif`
- Find **first non-zero** difference $i$ in `dif`, then start from $i+1$
- Loop `dif` compute `dif[i] * dif[i - 1]`
    - if < 0, it promise one more wiggle element, count++.
    - if > 0, it promise these 3 numbers are not wiggle(mono).
    - if == 0, there are two cases:
        - two are all zero
        - one of them is zero:
            - before flat, record the dif before flat as pre
            - after flat, compute `dif[i] * pre`

### Greedy #2 (improve)

- Overall: use preDiff and curDiff to determine a extreme point
    - preDiff = (nums[i] - nums[i - 1])
    - curDiff = (nums[i + 1] - nums[i])
    - no dif array! (less space cost)
    
    ```java
    preDiff = curDiff; // renew acoordingly
    ```
    
- **Edge case** handle:
    1. Plane between wiggle: only treat rear end as extreme point
    
    ```java
    (curDiff > 0 && preDiff <= 0) || (curDiff < 0 && preDiff >= 0)
    ```
    
    1. Head & tail: 
        - Head: assume preDiff before head is 0 → fit *case 1*
        - Tail: assume curDiff before tail makes **tail be a extreme point**
    2. Plane between monotonic:
        - need to record diff right before plane, and compare with curDiff right after the plane.
        - implement: renew preDiff only when curDiff ≠ 0 (within case1 if)

# Solution

## Greedy

### Raw

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        int len = nums.length;
        if (len == 1) {
            return 1;
        }

        // get differences
        int[] dif = new int[len - 1];
        for (int i = 0; i < dif.length; i++) { // len > 1
            dif[i] = nums[i + 1] - nums[i];
        }

        // dif.length >= 1
        int count = 1;
        int start = 1;

        for (int i = 0; i < dif.length; i++) {
            if (dif[i] != 0) {
                start += i;
                count++;
                break;
            }
        }

        int pre = 0;
        for (int i = start; i < dif.length; i++) { // dif.length >= 2
            int dot = dif[i] * dif[i - 1];

            if (dot < 0) {
                count ++;
            }

            if (dot == 0 && dif[i] != dif[i - 1]) {
                if (pre == 0) {
                    pre = dif[i - 1];
                } else {
                    if (dif[i] * pre < 0) {
                        count++;
                    }
                    
                    pre = 0;
                }                
            }
        }

        return count;
    }
}
```

### Cleaner ‘if’

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        int len = nums.length;
        if (len == 1) return 1;

        // get differences
        int[] dif = new int[len - 1];
        for (int i = 0; i < dif.length; i++) { // len > 1
            dif[i] = nums[i + 1] - nums[i];
        }

        // dif.length >= 1
        int count = 1;
        int start = 1;

        for (int i = 0; i < dif.length; i++) {
            if (dif[i] != 0) {
                start += i;
                count++;
                break;
            }
        }

        int pre = 0;
        for (int i = start; i < dif.length; i++) { // dif.length >= 2
            int dot = dif[i] * dif[i - 1];

            if (dot < 0) count++;

            if (dot == 0 && dif[i] != dif[i - 1]) {
                if (pre == 0) pre = dif[i - 1];
                else {
                    if (dif[i] * pre < 0) count++;
                    pre = 0;
                }                
            }
        }

        return count;
    }
}
```

### Carl’s

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length <= 1) {
            return nums.length;
        }
        //当前差值
        int curDiff = 0;
        //上一个差值
        int preDiff = 0;
        int count = 1;
        for (int i = 1; i < nums.length; i++) {
            //得到当前差值
            curDiff = nums[i] - nums[i - 1];
            //如果当前差值和上一个差值为一正一负
            //等于0的情况表示初始时的preDiff
            if ((curDiff > 0 && preDiff <= 0) || (curDiff < 0 && preDiff >= 0)) {
                count++;
                preDiff = curDiff;
            }
        }
        return count;
    }
}
```