# 538. Convert BST to Greater Tree

Date: Feb 17
Level: Medium
Minutes: 40
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree
Time Complexity: O(n)
Space Complexity: O(h)
URL: https://leetcode.com/problems/convert-bst-to-greater-tree/description/
Carl's: https://programmercarl.com/0538.%E6%8A%8A%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E8%BD%AC%E6%8D%A2%E4%B8%BA%E7%B4%AF%E5%8A%A0%E6%A0%91.html#%E6%80%9D%E8%B7%AF

<aside>
💡

Given the `root` of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus the sum of all keys greater than the original key in BST.

</aside>

# Thought:

- Recursion use a helper, with **reverse inorder order**
    - Design: return void, use only parameter `(TreeNode node)`.
        - Set `int sum` as global variable
    - At each step(division):
        - If node.right is not null, recurse
        - add root.val to sum, then let root.val = sum
        - If node.left is not null, recurse
    - Base case: leaf node.

# Solution

```java
class Solution {
    int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if (root == null) {
            return null;
        }

        convert(root);
        return root;
    }

    private void convert(TreeNode root) {
        if (root.right != null) {
            convert(root.right);
        }

        sum += root.val;
        root.val = sum;

        if (root.left != null) {
            convert(root.left);   
        }
    }
}
```