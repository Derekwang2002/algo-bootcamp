# 503. Find Mode in  Binary Search Tree

Date: Feb 14
Minutes: 60
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

<aside>
💡

Given the `root` of a binary search tree (BST) with duplicates, return *all the [mode(s)](https://en.wikipedia.org/wiki/Mode_(statistics)) (i.e., the most frequently occurred element) in it*.

If the tree has more than one mode, return them in **any order**.

</aside>

# Thought:

- BST, use its nature
- Nodes with same value must be adjacent, so by inorder traversal we can get count of current value and compare with a former modes count to **maintain modes list**.
    - Use global `count` and `maxCount` to record current count and modes count
    - Use global `prev`(double pointer) to record `TreeNode` last visited, so we can check if current node is “next value”,
- At each step (for middle node), `count++`,
    - if `root.val ≠ prev.val`, it means moved to “next value”.
        - renew `count = 1`
    - If `count > maxCount`
        - renew `maxCount = count`
        - add value to modes list.
    - If `count == maxCount`
        - add value to modes list.
    - renew `prev = root`.

# Solution

## Inorder

```java
class Solution {
    TreeNode prev = null;
    int count = 0;
    int maxCount = 0;

    public int[] findMode(TreeNode root) {
        List<Integer> modes = new ArrayList<>();
        if (root != null) {
            modeBST(root, modes);
        }

        int n = modes.size();
        int[] result = new int[n];
        for (int i = 0; i < n; i++) {
            result[i] = modes.get(i);
        }
        
        return result;
    }

    private void modeBST(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }

        modeBST(root.left, res); // pre

        count++; //mid
        
        if (prev != null && prev.val != root.val) { 
            count = 1;
        }

        prev = root;

        if (count > maxCount) {
            res.clear();
            res.add(root.val);
            maxCount = count;
        } else if (count == maxCount) {
            res.add(root.val);
        }

        modeBST(root.right, res); // post
    }
}
```