# 102. Binary Tree Level Order Traversal

Date: Feb 9
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

# Thought:

- The order of **BFS** satisfied charateristic of queue
- When polling a node, add its childs (left to right)
- Use previous queue length to control each row traversing

# Solution

## Raw

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;

        List<Integer> resRow = new ArrayList<>();
        List<TreeNode> nodeRow = new ArrayList<>();
        nodeRow.add(root);
        resRow.add(root.val);
        // res.add(Arrays.asList(root.val));

        while (nodeRow.size() > 0) {
            List<TreeNode> row = new ArrayList<>();
            res.add(resRow);
            resRow = new ArrayList<>();

            for (TreeNode n : nodeRow) {
                if (n.left != null) {
                    row.add(n.left);
                    resRow.add(n.left.val);
                }

                if (n.right != null) {
                    row.add(n.right);
                    resRow.add(n.right.val);
                }
            }

            nodeRow = row;
        }

        return res;
    }
}
```

## Standard method - by queue and recursion

```java
// 102.二叉树的层序遍历
class Solution {
    public List<List<Integer>> resList = new ArrayList<List<Integer>>();

    public List<List<Integer>> levelOrder(TreeNode root) {
        //checkFun01(root,0);
        checkFun02(root);

        return resList;
    }

    //BFS--递归方式
    public void checkFun01(TreeNode node, Integer deep) {
        if (node == null) return;
        deep++;

        if (resList.size() < deep) {
            //当层级增加时，list的Item也增加，利用list的索引值进行层级界定
            List<Integer> item = new ArrayList<Integer>();
            resList.add(item);
        }
        resList.get(deep - 1).add(node.val);

        checkFun01(node.left, deep);
        checkFun01(node.right, deep);
    }

    //BFS--迭代方式--借助队列
    public void checkFun02(TreeNode node) {
        if (node == null) return;
        Queue<TreeNode> que = new LinkedList<TreeNode>();
        que.offer(node);

        while (!que.isEmpty()) {
            List<Integer> itemList = new ArrayList<Integer>();
            int len = que.size();

            while (len > 0) {
                TreeNode tmpNode = que.poll();
                itemList.add(tmpNode.val);

                if (tmpNode.left != null) que.offer(tmpNode.left);
                if (tmpNode.right != null) que.offer(tmpNode.right);
                len--;
            }

            resList.add(itemList);
        }

    }
}
```

## My queue

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;

        Queue<TreeNode> que = new ArrayDeque<>();
        que.offer(root);
        TreeNode cur;

        while (!que.isEmpty()) {
            int len = que.size();
            List<Integer> resRow = new ArrayList<>();

            while (len > 0) {
                cur = que.poll();
                resRow.add(cur.val);

                if (cur.left != null) {
                    que.offer(cur.left);
                }

                if (cur.right != null) {
                    que.offer(cur.right);
                }

                len--;
            }

            res.add(resRow);
        }

        return res;
    }
}
```