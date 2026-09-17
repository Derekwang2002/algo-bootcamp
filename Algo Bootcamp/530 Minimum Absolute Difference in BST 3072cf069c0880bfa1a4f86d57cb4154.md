# 530. Minimum Absolute Difference in BST

Date: Feb 13
Minutes: 60
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

<aside>
💡

Given the `root` of a Binary Search Tree (BST), return *the minimum absolute difference between the values of any two different nodes in the tree*.

</aside>

# Thought:

- Since its BST, we can do inorder traverse and use double pointer to compare min difference
    - Min differnce in sorted array will only between two neighbours.

# Solution

## BST

```java
class Solution {
    int min = Integer.MAX_VALUE;
    TreeNode prev = null;

    public int getMinimumDifference(TreeNode root) {
        setMin(root);
        return min;
    }

    private void setMin(TreeNode root) {
        if (root.left != null) {
            setMin(root.left);
        }

        if (prev != null) {
            min = Math.min(min, root.val - prev.val);
        }
        
        prev = root;

        if (root.right != null) {
           setMin(root.right);
        }
    }
}
```

## regular tree

```java
class Solution {
    public int getMinimumDifference(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        getList(root, list);
        list.sort(null);

        int l = 0, r = 1;
        int min = Integer.MAX_VALUE;

        while (r < list.size()) {
            int gap = Math.abs(list.get(l++) - list.get(r++));
            min = Math.min(min, gap);
        }

        return min;
    }

    private void getList(TreeNode root, List<Integer> list) {
        if (root == null) {
            return;
        }

        list.add(root.val);
        getList(root.left, list);
        getList(root.right, list);
    }
}
```

## Carl’s

```java
class Solution {
    TreeNode pre; // 记录上一个遍历的结点
    int result = Integer.MAX_VALUE;

    public int getMinimumDifference(TreeNode root) {
        if (root == null)
            return 0;
        traversal(root);
        return result;
    }

    public void traversal(TreeNode root) {
        if (root == null)
            return;
        // 左
        traversal(root.left);
        // 中
        if (pre != null) {
            result = Math.min(result, root.val - pre.val);
        }
        pre = root;
        // 右
        traversal(root.right);
    }
}
```