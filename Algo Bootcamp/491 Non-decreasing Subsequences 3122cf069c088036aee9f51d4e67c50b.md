# 491. Non-decreasing Subsequences

Date: Feb 25
Level: Medium
Minutes: 35
Review Recommendation: ✅ Good job
Status: Done
Tags: Backtracking
Time Complexity: O(n * 2^n)
Space Complexity: O(n)
URL: https://leetcode.com/problems/non-decreasing-subsequences/
Carl's: https://programmercarl.com/0491.%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given an integer array `nums`, return *all the different possible non-decreasing subsequences of the given array with at least two elements*. You may return the answer in **any order**.

</aside>

# Thought

- Based on [**78. Subsets**](78%20Subsets%203122cf069c0880f3bd32c02588279a51.md) but has more constrains:
    - Elements in the given array may duplicate
    - Results depend on the origin order(so cannot do presort)
    - Only return non-decreasing subsequence
- Based on *subset*, we need do 2 trims:
    - **trim** decreasing subsequence(underlying requirement, length>1)
    
    ```java
    boolean isDcrs = (len != 0) && nums[i] < path.get(len - 1);
    ```
    
    - **trim** duplication in the same level(`used` is a hash set defined before loop)
    
    ```java
    boolean isDup = !used.add(nums[i]);
    ```
    

# Solution

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new ArrayList<>();

    public List<List<Integer>> findSubsequences(int[] nums) {
        backtrack(nums, 0);
        return res;
    }

    private void backtrack(int[] nums, int idx) {
        Set<Integer> used = new HashSet<>(); 
        // faster than use res as a hash set

        for (int i = idx; i < nums.length; i++) {
            int len = path.size();
            
            boolean isDcrs = len != 0 && nums[i] < path.get(len - 1);
            boolean isDup = !used.add(nums[i]);

            if (isDup || isDcrs) {
                continue;
            }

            path.add(nums[i]); // len + 1

            if (len != 0) {
                res.add(new ArrayList<>(path));
            }
            
            backtrack(nums, i + 1);
            path.remove(len);
        }
    }
}
```

## Improve

```java
class Solution {
    public List<List<Integer>> findSubsequences(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(nums, 0, new ArrayList<>(), res);
        return res;
    }

    private void backtrack(int[] nums, int start, List<Integer> path, List<List<Integer>> res) {
        if (path.size() >= 2) {
            res.add(new ArrayList<>(path));
        }
        
        // 使用数组代替 HashSet，范围 [-100, 100] -> 偏移 100 映射到 [0, 200]
        boolean[] used = new boolean[201]; 
        
        for (int i = start; i < nums.length; i++) {
            // 1. 非递减检查
            if (!path.isEmpty() && nums[i] < path.get(path.size() - 1)) continue;
            // 2. 同层去重检查
            if (used[nums[i] + 100]) continue;
            
            used[nums[i] + 100] = true;
            path.add(nums[i]);
            backtrack(nums, i + 1, path, res);
            path.remove(path.size() - 1);
        }
    }
}
```