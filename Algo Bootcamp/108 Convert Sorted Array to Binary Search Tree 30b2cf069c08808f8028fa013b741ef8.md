# 108. Convert Sorted Array to Binary Search Tree

Date: Feb 17
Level: Easy
Minutes: 15
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree
Time Complexity: O(n)
Space Complexity: O(h)
URL: https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/
Carl's: https://programmercarl.com/0108.%E5%B0%86%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given an integer array `nums` where the elements are sorted in **ascending order**, convert *it to a **height-balanced*** *binary search tree*.

</aside>

# Thought:

- Recursion
    - Design: return TreeNode, use paras `(int[] nums, int head, int tail)`
        - `head` and `tail` → virtually divide the `nums` → current range
        - return middle value as node
    - Division: use value with index `mid=(head+tail)/2` as root node value
        - root.left → recursion on left side nums
        - root.right → recursion on right side nums
    - Base Case: if head > tail, return null.

# Solution

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return build(nums, 0, nums.length - 1);

    }

    private TreeNode build(int[] nums, int head, int tail) { //[] range
        if (head > tail) {
            return null;
        }

        int mid = (head + tail) / 2;
        TreeNode root = new TreeNode(nums[mid]);

        root.left = build(nums, head, mid - 1);
        root.right = build(nums, mid + 1, tail);

        return root;
    }
}
```