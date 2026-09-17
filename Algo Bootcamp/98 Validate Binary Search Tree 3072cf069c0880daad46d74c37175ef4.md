# 98. Validate Binary Search Tree

Date: Feb 13
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

<aside>
💡

Given the `root` of a binary tree, *determine if it is a valid binary search tree (BST)*.

A **valid BST** is defined as follows:

- The left  of a node contains only nodes with keys **strictly less than** the node's key.
    
    subtree
    
- The right subtree of a node contains only nodes with keys **strictly greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.
</aside>

# Thought:

- Actually is ot varify inorder list of the treee is sorted
    - By inorder comparing mid and previous.
- Fact: BST’s inorder list is sorted

# Solution

## Raw

```java
class Solution {
    List<Integer> inorder = new ArrayList<>();

    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }

        if (!isValidBST(root.left)) {
            return false;
        }

        int n = inorder.size();
        if (n > 0 && root.val <= inorder.get(n - 1)) {
            return false;
        }

        inorder.add(root.val);

        if (!isValidBST(root.right)) {
            return false;
        }

        return true;
    }
}
```

## Optimal in memory complexity

```java
class Solution {
    long prev = Long.MIN_VALUE;

    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }

        if (!isValidBST(root.left)) {
            return false;
        }

        if (root.val <= prev) {
            return false;
        }

        prev = root.val;

        return isValidBST(root.right);
    }
}
```

## Carl’s

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return validBST(Long.MIN_VALUE, Long.MAX_VALUE, root);
    }
    boolean validBST(long lower, long upper, TreeNode root) {
        if (root == null) return true;
        if (root.val <= lower || root.val >= upper) return false;
        return validBST(lower, root.val, root.left) && validBST(root.val, upper, root.right);
    }
}
```