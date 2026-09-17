# 404. Sum of Left Leaves

Date: Feb 11
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

<aside>
💡

Given the `root` of a binary tree, return *the sum of all left leaves.*

A **leaf** is a node with no children. A **left leaf** is a leaf that is the left child of another node.

</aside>

# Thought:

- Postorder travsersal, return sum of childs `sumOfLeftLeaves()` at each step
- Need a helper function with a `TreeNode`, and a `isLeft` **boolean** parameter to tell if current node is at the left side.
- Base case: if it’s at left, and is leaf node, return node.val
- If it’s right leaf, it will return 0 (default value).

# Solution

## Postorder + helper

```
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        return leftLeaves(root, false);
    }

    private int leftLeaves(TreeNode root, boolean isLeft) {
        if (root.left == null && root.right == null && isLeft) {
            return root.val;
        }

        int left = 0, right = 0;

        if (root.left != null) {
            left = leftLeaves(root.left, true);
        }

        if (root.right != null) {
            right  = leftLeaves(root.right, false);
        }

        return left + right;
    }
}
```

## Carl’s

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if (root == null) return 0;
        int leftValue = sumOfLeftLeaves(root.left);    // 左
        int rightValue = sumOfLeftLeaves(root.right);  // 右

        int midValue = 0;
        if (root.left != null && root.left.left == null && root.left.right == null) {
            midValue = root.left.val;
        }
        int sum = midValue + leftValue + rightValue;  // 中
        return sum;
    }
}
```

## Iteration

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if (root == null) return 0;
        Stack<TreeNode> stack = new Stack<> ();
        stack.add(root);
        int result = 0;
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            if (node.left != null && node.left.left == null && node.left.right == null) {
                result += node.left.val;
            }
            if (node.right != null) stack.add(node.right);
            if (node.left != null) stack.add(node.left);
        }
        return result;
    }
}
```

(BFS could also work: if left child is leaf, add on, no row control)