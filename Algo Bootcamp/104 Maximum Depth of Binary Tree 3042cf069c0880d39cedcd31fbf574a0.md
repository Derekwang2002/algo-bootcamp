# 104. Maximum Depth of Binary Tree

Date: Feb 10
Minutes: 40
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

<aside>
💡

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

</aside>

Timer: ***20min &*** ***20min for DFS & recursion***

# Thought:

- **Depth / DFS: (actually a preorder)**
    - use **preorder** traverse, use a stack of `Pair<TreeNode, Integer>` to store right child and their current depth.
- **Height / Recursion: (actually a postorder)**
    - base case: node is null, return 0 (*enough*); node has no childs, return 1 (this is useless)
    - Value Pass Design: return type is int, in each non stop recurison, return **max of (left max & right max) + 1**
- Evalutation:
    - Height: distance from current node to the lowest leaf node
    - Depth: distance from current node to the root node
    - For stop condition, this is enough: (if childs are null, it’s 0+1)
    
    ```java
    if (root == null) { return 0; }
    ```
    
    - In each recursion, no need to determine if `cur.left/right` are null, since when we reach that node, we already have a stop determination for it.

# Solution

## Bad performance

```java
class Solution {
    public int maxDepth(TreeNode root) {
        int max = 0;
        int count = 0;
        Deque<Pair<TreeNode, Integer>> stack = new ArrayDeque<>();
        TreeNode cur = root;

        while (!stack.isEmpty() || cur != null) {
            while (cur != null) {
                count++;
                if (cur.right != null) {
                    stack.push(new Pair<>(cur.right, count));
                }

                cur = cur.left;
            }

            max = Math.max(count, max);

            if (stack.peek() != null) {
                cur = stack.peek().getKey();
                count = stack.peek().getValue();
                stack.pop();
            }
        }

        return max;
    }
}
```

## My recursion

```java
class Solution {
    public int maxDepth(TreeNode root) {
        int left, right;
        left = right = 0;

        if (root == null) {
            return 0;
        }

        if (root.left == null && root.right == null) { // useless
            return 1;
        }

        if (root.left != null) { // useless 
            left =  maxDepth(root.left);
        }

        if (root.right != null) { // useless 
            right = maxDepth(root.right);
        }

        return Math.max(left, right) + 1;
    }
}
```

## Ultra clean

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) { return 0; }
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```