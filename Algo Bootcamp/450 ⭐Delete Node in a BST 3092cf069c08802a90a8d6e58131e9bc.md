# 450. ⭐Delete Node in a BST

Date: Feb 15
Minutes: 120
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

<aside>
💡

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return *the **root node reference** (possibly updated) of the BST*.

Basically, the deletion can be divided into two stages:

1. Search for a node to remove.
2. If the node is found, delete the node.
</aside>

# Thought:

- Two steps: search key + delete node if found.
    - After found key value, then substitute it with a appropriate value of its subtree.
- In BST, when subsitute a node, it can do from both side.
    - suppose we do from **right side**.
- Cases:
    1. key not found: return origin tree.
    2. found, no substitution(on right side): return left side node.
    3. found, need substitution: find the **min value** of the subtree with `key-value-node.right` as root.(closet to the node deleted).
- Recursion design: (one recursion to search, another to delete)
    - Intention: Search recursion trivial, the delete recursion is actually to find the min value of right side subtree.
    - Return type: both are TreeNode
    - Combination: return current node at each step. By doing so, we can automatically delete node we want(just return next node).
    - Base case:
        - deleting recursion: min-value node(node.left is null), return node.right
        - searching recursion(main):
            - found key: case2 + case3
            - null node: case1

# Solution

```java
class Solution {
    int sub;

    public TreeNode deleteNode(TreeNode root, int key) {
        // edge case (empty tree or no key found)
        if (root == null) {
            return root;
        }

        // found -> delete
        if (root.val == key) {
            if (root.right != null) { // has substitute on right
                root.right = minBST(root.right); // delete node within recursion
                root.val = sub;
            } else { // no right or leaf
                root = root.left; // set current to left for return (delete)
            }
            return root;
        }
        
        // search
        if (root.val > key) {
            root.left = deleteNode(root.left, key);
        }
        if (root.val < key) {
            root.right = deleteNode(root.right, key);
        }

				// give current back upwards
        return root; 
    }

    private TreeNode minBST(TreeNode root) { // right side         
        if (root.left == null) { 
            sub = root.val;
            return root.right; // remove nodes
        }

        root.left = minBST(root.left); // recursion
        return root;
    }
}
```