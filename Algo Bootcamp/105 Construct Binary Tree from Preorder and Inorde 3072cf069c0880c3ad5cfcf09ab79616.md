# 105. Construct Binary Tree from Preorder and Inorder Traversal

Date: Feb 12
Minutes: 5
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

# Thought:

- Similar to [106. ⭐Construct Binary Tree from Inorder and Postorder Traversal ](106%20%E2%AD%90Construct%20Binary%20Tree%20from%20Inorder%20and%20Postor%203062cf069c0880b5a475d41b0728ef60.md) , simply change traverse order is okay.

# Solution

```java
class Solution {
    int[] preorder;
    int rootIndex = 0;
    Map<Integer, Integer> map;

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int n = preorder.length;
        this.preorder = preorder;

        map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            map.put(inorder[i], i);
        }

        return build(inorder, 0, n - 1);
    }

    private TreeNode build(int inL, int inR) {
        if (inL > inR) {
            return null;
        }

        int rootValue = preorder[rootIndex++];
        TreeNode root = new TreeNode(rootValue);
        int mid = map.get(rootValue);

        root.left = build(inL, mid - 1);
        root.right = build(mid + 1, inR);

        return root;
    }
}
```