# 226. Invert Binary Tree

Date: Feb 10
Minutes: 15
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

<aside>
💡

Given the `root` of a binary tree, invert the tree, and return *its root*.

</aside>

# Thought:

- Do BFS without row size control
- Invert childs of every polled out nodes,
- if childs are not null, push in queue.
- Evaluation:
    - According to traversal order, solution may vary
        - Inorder recursion need extra modification
        
        ```cpp
        class Solution {
        public:
            TreeNode* invertTree(TreeNode* root) {
                if (root == NULL) return root;
                invertTree(root->left);         // 左
                swap(root->left, root->right);  // 中
                invertTree(root->left);         // 注意 依然遍历左孩子，因为中间节点已翻转
                return root;
            }
        };
        ```
        
    - But the core thought is: swap childs when popping a node
    - Detail explaination
    
    [226.翻转二叉树](https://programmercarl.com/0226.%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E6%80%9D%E8%B7%AF)
    

# Solution

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        Queue<TreeNode> que = new ArrayDeque<>();
        if (root != null) {
            que.offer(root);
        }

        while (!que.isEmpty()) {
            TreeNode cur = que.poll();
            TreeNode temp = cur.left;
            cur.left = cur.right;
            cur.right = temp;

            if (cur.left != null) {
                que.offer(cur.left);
            }

            if (cur.right != null) {
                que.offer(cur.right);
            }
        }

        return root;
    }
}
```

## Recursion

```java
//DFS递归
class Solution {
   /**
     * 前后序遍历都可以
     * 中序不行，因为先左孩子交换孩子，再根交换孩子（做完后，右孩子已经变成了原来的左孩子），再右孩子交换孩子（此时其实是对原来的左孩子做交换）
     */
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        
        invertTree(root.left);
        invertTree(root.right);
        swapChildren(root);
        return root;
    }

    private void swapChildren(TreeNode root) {
        TreeNode tmp = root.left;
        root.left = root.right;
        root.right = tmp;
    }
}
```