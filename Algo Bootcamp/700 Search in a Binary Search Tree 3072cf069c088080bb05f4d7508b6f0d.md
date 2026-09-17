# 700. Search in a Binary Search Tree

Date: Feb 13
Minutes: 5
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

# Thought:

- Nature of BST

# Solution

```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null || root.val == val) {
            return root;
        }
        
        if (val < root.val){
            return searchBST(root.left, val);
        } else {
            return searchBST(root.right, val);
        }
    }
}
```

## Iterative

```java
class Solution {
    // 迭代，利用二叉搜索树特点，优化，可以不需要栈
    public TreeNode searchBST(TreeNode root, int val) {
        while (root != null)
            if (val < root.val) root = root.left;
            else if (val > root.val) root = root.right;
            else return root;
        return null;
    }
}
```