# 513. Find Bottom Left Tree Value

Date: Feb 11
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

<aside>
💡

Given the `root` of a binary tree, return the leftmost value in the last row of the tree.

</aside>

# Thought:

- **BFS with row control:** renew fisrt node of the current row, at last return value of the node.
- **DFS with left first:**
    - Need use two global variables `result, maxDepth` to record
    - Use helper func with void return and paras `TreeNode, depth`
    - At each node, depth++, compare it with maxDepth, if bigger and also a leaf node, renew result and maxDepth(depth **backtracking**)
    - Note: *no need to determine if it’s left node*, since we traverse left first, so right node will never deeper than maxDepth.

# Solution

## BFS with row control

```java
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> que = new ArrayDeque<>();
        que.offer(root);

        TreeNode leftMost = root;

        while (!que.isEmpty()) {
            int len = que.size();
            leftMost = que.peek();

            while (len-- > 0) {
                TreeNode cur = que.poll();

                if (cur.left != null) {
                    que.offer(cur.left);
                }

                if (cur.right != null) {
                    que.offer(cur.right);
                }
            }
        }

        return leftMost.val;
    }
}
```

## DFS with left first

```java
class Solution {
    int maxD = 0;
    int result;

    public int findBottomLeftValue(TreeNode root) {
        bottomLeft(root, 0);
        return result;
    }
    
    private void bottomLeft(TreeNode root, int d) {
        int temp = d;
        d++;

        boolean isLeaf = root.left == null && root.right == null;
        if (d > maxD && isLeaf) {
            maxD = d;
            result = root.val;
        }

        if (root.left != null) {
            bottomLeft(root.left, d);
        }

        if (root.right != null) {
            bottomLeft(root.right, d);
        }

        d = temp; // back tracking
    }
}
```