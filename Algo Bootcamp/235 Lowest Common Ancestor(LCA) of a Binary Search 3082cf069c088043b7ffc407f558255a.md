# 235. Lowest Common Ancestor(LCA) of a Binary Search Tree

Date: Feb 14
Minutes: 15
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

<aside>
💡

Given a binary search tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

</aside>

# Thought:

- Simmilar to [236. ⭐Lowest Common Ancestor(LCA) of a Binary Tree](236%20%E2%AD%90Lowest%20Common%20Ancestor(LCA)%20of%20a%20Binary%20Tree%203082cf069c0880eea60af01ae13e0d45.md)
- *Use nature of BST, t*he only difference is the condition determination of LCA changed: **weather current value is between two target nodes’ value**: (then return root)
    - if (low < root.val && root.val < high)
    - else if (root.val == low || root.val == high)

# Soltuion

## Use BST nature

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode low = q;
        TreeNode high = p;

        if (p.val < q.val) {
            low = p;
            high = q;
        }

        return lcaBST(root, low.val, high.val);
    }

    private TreeNode lcaBST(TreeNode root, int low, int high) {
        if (root == null) {
            return null;
        }

        if (low < root.val && root.val < high) {
            return root;
        }

        if (root.val == low || root.val == high) {
            return root;
        }

        TreeNode left = lcaBST(root.left, low, high);
        TreeNode right = lcaBST(root.right, low, high);

        if (left != null) {
            return left;
        } 
        
        if (right != null) {
            return right;
        }

        return null;
    }
}
```

## Less code

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root.val > p.val && root.val > q.val) {
		        return lowestCommonAncestor(root.left, p, q);
        }
        if (root.val < p.val && root.val < q.val) {
		        return lowestCommonAncestor(root.right, p, q);
        }
        return root;
    }
}
```