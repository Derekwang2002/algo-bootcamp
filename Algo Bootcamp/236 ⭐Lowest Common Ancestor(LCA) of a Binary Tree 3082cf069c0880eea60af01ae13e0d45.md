# 236. ⭐Lowest Common Ancestor(LCA) of a Binary Tree

Date: Feb 14
Minutes: 40
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

<aside>
💡

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

</aside>

# Thought:

- **Raw method**: Find path for two nodes, and get last nodes that are same.
    - DFS get path helper function.
    - not optimal, slow/space consuming.
- **Optimal method**: postorder, **find 1st node has p&q in its subtree.**
    - base case: if root is null return null; if root.val equals to p/q, return p/q.
    - middle process:
        1. case1: if left&right recusion return not null, return root as LCA.
        2. case2: if one of left/right is null and another not null, return the non-null one (LCA founded).
        3. case3: left&right recursion return are all null, return null, which means no p&q in its subtree.
    - *Special situation*: what if one of p/q is the LCA?
        - it will fall in **base case + case2**, the returned node is LCA itself.

# Solution

## Raw

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        List<TreeNode> pathP = new ArrayList<>();
        List<TreeNode> pathQ = new ArrayList<>();

        getPath(root, p, pathP);
        getPath(root, q, pathQ);
        int lcaIndex = 0;

        for (int i = 0; i < Math.min(pathQ.size(), pathP.size()); i++) {
            if (pathP.get(i).val != pathQ.get(i).val) {
                break;
            }

            lcaIndex = i;
        }

        return pathQ.get(lcaIndex);
    }

    private boolean getPath(TreeNode root, TreeNode node, List<TreeNode> path) {
        if (root == null) {
            return false;
        }

        int len = path.size();
        path.add(root);

        if (root.val == node.val) {
            return true;
        }

        if (getPath(root.left, node, path)) {
            return true;
        }
        
        if (getPath(root.right, node, path)) {
            return true;
        }

        while (path.size() > len) { // backtracking
            path.remove(path.size() - 1);
        }

        return false;
    }
}
```

## Optimal

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root.val == p.val || root.val == q.val) {
            return root; // since values are unique;
        }

        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        if (left != null && right != null) {
            return root;
        } else if (left != null) {
            return left;
        } else if (right != null) {
            return right;
        }

        return null;
    }
}
```