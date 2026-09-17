# 222. Count Complete Tree Nodes

Date: Feb 10
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

# Thought:

- Count like a random BT, traverse and record count.
- Use **complete BT nature**:[**Complete BT**: first n nodes of a Full BT, or **composed by Full BTs**!](https://app.notion.com/p/Complete-BT-first-n-nodes-of-a-Full-BT-or-composed-by-Full-BTs-3022cf069c0880259f67dfbb6ee8c952?pvs=21)
    - Composed of FULL BT → **Recursion with FULL BT base case**
    - Return int, and check left/right subtree in each recursion.
    - **Time**：$O(log n) < O(log^2 n) << O(n)$
    - Space：$O(log n)$

# Solution

```java
class Solution {
    public int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }

        TreeNode left = root.left, right = root.right;
        int leftH = 1, rightH = 1;

        while (left != null) {
            left = left.left;
            leftH++;
        }

        while (right != null) {
            right = right.right;
            rightH++;
        }

        if (leftH == rightH) {
            return (int) Math.pow(2, leftH) - 1;
        }

        return countNodes(root.left) + countNodes(root.right) + 1;
    }
}
```

- 时间复杂度：$O(log n × log n)$
- 空间复杂度：$O(log n)$