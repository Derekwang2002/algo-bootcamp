# 111. Minimum Depth of Binary Tree

Date: Feb 10
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

<aside>
💡

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

</aside>

# Thought:

- Recursion:
    - Different from maxDepth, minDepth need to detect leaf node
    - Method 1: when current node is leaf, return 1
    - Method 2: if only one child is null, return non-null side + 1
- Iteration:
    - Do BFS with row control, then return at first leaf.

# Solution

## Mine

```java
class Solution {
    public int minDepth(TreeNode root) {
        int left, right;
        left = right = 100001;

        if (root == null) { // avoid root is null
            return 0;
        }

        if (root.left == null && root.right == null) { // base case
            return 1;
        }

        if (root.left != null) { 
            left =  minDepth(root.left);
        }

        if (root.right != null) { 
            right = minDepth(root.right);
        }

        return Math.min(left, right) + 1;
    }
}
```

## cleaner way - carl’s

```java
class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int leftDepth = minDepth(root.left);
        int rightDepth = minDepth(root.right);
        
        if (root.left == null) {
            return rightDepth + 1;
        }
        
        if (root.right == null) {
            return leftDepth + 1;
        }
        
        return Math.min(leftDepth, rightDepth) + 1;
    }
}
// ultra clean
class Solution {
    public int minDepth(TreeNode root) {

        if (root == null) { // base case
            return 0;
        }

        if (root.left == null && root.right != null) { 
            return minDepth(root.right) + 1;
        }

        if (root.right == null && root.left != null) { 
            return minDepth(root.left) + 1;
        }

        return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
    }
}
```

## My Iterative (best time efficiency)

```java
class Solution {
    public int minDepth(TreeNode root) {
        Queue<TreeNode> que = new ArrayDeque<>();
        int count = 0; 

        if (root != null) {
            que.offer(root);
        }

        while (!que.isEmpty()) {
            int len = que.size();
            count++;

            while (len-- > 0) {
                TreeNode cur = que.poll();

                if (cur.left == null && cur.right == null) { // return at first leaf
                    return count;
                }

                if (cur.left != null) {
                    que.offer(cur.left);
                }

                if (cur.right != null) {
                    que.offer(cur.right);
                }
            }
        }

        return count;
    }
}
```