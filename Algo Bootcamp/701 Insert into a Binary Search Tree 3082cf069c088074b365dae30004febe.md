# 701. Insert into a Binary Search Tree

Date: Feb 15
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

<aside>
💡

You are given the `root` node of a binary search tree (BST) and a `value` to insert into the tree. Return *the root node of the BST after the insertion*. It is **guaranteed** that the new value does not exist in the original BST.

**Notice** that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return **any of them**.

</aside>

# Thought:

- Suppose val is in tree, and search it, when meet null, we found the position to insert val.
- Iteration → Double pointer
- Recursion → base case: return val node

# Solution

## Raw

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        TreeNode target = new TreeNode(val);
        TreeNode cur = root;

        if (root == null) {
            return target;
        }

        while (cur != target) {
            if (val < cur.val) {
                if (cur.left == null) {
                    cur.left = target;
                }
                cur = cur.left;
            }

            if (cur.val < val) {
                if (cur.right == null) {
                    cur.right = target;
                }
                cur = cur.right;
            }
        }

        return root;
    }
}
```

## Modified

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) return new TreeNode(val);
        TreeNode newRoot = root;
        TreeNode pre = root;
        while (root != null) {
            pre = root;
            if (root.val > val) {
                root = root.left;
            } else if (root.val < val) {
                root = root.right;
            }
        }
        if (pre.val > val) {
            pre.left = new TreeNode(val);
        } else {
            pre.right = new TreeNode(val);
        }

        return newRoot;
    }
}
```

## Recursion(less code)

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) // 如果当前节点为空，val找到了合适的位置，此时创建节点直接返回。
            return new TreeNode(val);

        if (root.val < val){
            root.right = insertIntoBST(root.right, val); // 递归inssert右子树
        }else if (root.val > val){
            root.left = insertIntoBST(root.left, val); // 递归创inssert左子树
        }
        return root;
    }
}
```