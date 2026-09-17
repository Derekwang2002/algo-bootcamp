# 669. Trim a Binary Search Tree

Date: Feb 17
Level: Medium
Minutes: 40
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree
Time Complexity: O(n)
Space Complexity: O(h)
URL: https://leetcode.com/problems/trim-a-binary-search-tree/description/
Carl's: https://programmercarl.com/0669.%E4%BF%AE%E5%89%AA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html

<aside>
💡

Given the `root` of a binary search tree and the lowest and highest boundaries as `low` and `high`, trim the tree so that all its elements lies in `[low, high]`. Trimming the tree should **not** change the relative structure of the elements that will remain in the tree (i.e., any node's descendant should remain a descendant). It can be proven that there is a **unique answer**.

Return *the root of the trimmed binary search tree*. Note that the root may change depending on the given bounds.

</aside>

# Thought:

- Inorder Recursion
    - Design: Reconstruct BST by passing TreeNode
        - No helper need
        - Return type TreeNode
    - Steps:
        - let node.left = recursion result of node.left
        - let node.right = recursion result of node.right
        - Check for return:
            1. If a node < low
                - All its subnode on left must < low.
                - Return node.right
            2.  If a node > hight
                - All utse subnode on right must > high
                - Return node.left
            3. Valid case:
                1. Return its self(node)
- **Evaluation: *Nailed it in one go*!**

# Solution

```java
class Solution {
    TreeNode prev = null;
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) {
            return null;
        }

        root.left = trimBST(root.left, low, high);

        root.right = trimBST(root.right, low, high);

        if (root.val < low) {
            return root.right;
        }

        if (root.val > high) {
            return root.left;
        }

        return root;
    }
}
```

## Improved

```java
public TreeNode trimBST(TreeNode root, int low, int high) {
    if (root == null) return null;

    // If root is too small, the whole left side is too small. 
    // Just move to the right.
    if (root.val < low) return trimBST(root.right, low, high);

    // If root is too big, the whole right side is too big. 
    // Just move to the left.
    if (root.val > high) return trimBST(root.left, low, high);

    // If root is in range, connect trimmed children
    root.left = trimBST(root.left, low, high);
    root.right = trimBST(root.right, low, high);

    return root;
}
```