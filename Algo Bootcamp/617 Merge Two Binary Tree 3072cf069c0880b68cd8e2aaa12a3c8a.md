# 617. Merge Two Binary Tree

Date: Feb 13
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

<aside>
💡

You are given two binary trees `root1` and `root2`.

Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. You need to merge the two trees into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of the new tree.

Return *the merged tree*.

**Note:** The merging process must start from the root nodes of both trees.

</aside>

# Thought:

- DFS traverse an imagine tree that contains all nodes of overlapping.
- At each step, new `root`, and set `root.val` accordingly, if one of the nodes are null, set the null nodes as a dummy nodes(global, val=0)
- Base case: both two nodes are null, return null.
- Evaluation:
    - Two time consuming process in raw solution:
        1. new TreeNode at each step; 
        2. keep goes down when one of nodes are null(should’ve returned) 
    - If one node is null, we return ***another ndoe*** immediately, because even if ***anthoer node*** is not null, we don’t need to do merge on it. So by returnning ***another node***, we got the whole subtree directly.

# Solution

## Raw

```java
class Solution {
    TreeNode fakeNode = new TreeNode(0);

    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        TreeNode root = new TreeNode();

        if (root1 == null && root2 == null) {
            return null; // base case
        } else if (root1 == null) {
            root1 = fakeNode;
            root.val = root2.val;
        } else if (root2 == null) {
            root2 = fakeNode;
            root.val = root1.val;
        } else { // (root1 != null && root2 != null)
            root.val = root1.val + root2.val;
        }

        root.left = mergeTrees(root1.left, root2.left);
        root.right = mergeTrees(root1.right, root2.right);

        return root;
    }
}
```

## Optimal

```java
class Solution {
    // 递归
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null) return root2;
        if (root2 == null) return root1;

        root1.val += root2.val;
        root1.left = mergeTrees(root1.left, root2.left);
        root1.right = mergeTrees(root1.right, root2.right);
        return root1;
    }
}
```