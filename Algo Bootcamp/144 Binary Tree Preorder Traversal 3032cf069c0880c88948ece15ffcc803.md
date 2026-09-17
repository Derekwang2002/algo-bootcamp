# 144. Binary Tree Preorder Traversal

Date: Feb 9
Minutes: 50
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree
URL: https://leetcode.com/problems/binary-tree-preorder-traversal/description/

# Thought

- Recursive: Use one helper function, take a list to store result, and control order
- Iterative: Asynchronous visit node and add result (maintain stack)

# Solution

## Iterative

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode cur = root;

        while (!stack.isEmpty() || cur != null) {
            while (cur != null) {
                res.add(cur.val);

                if (cur.right != null) {
                    stack.push(cur.right);
                }

                cur = cur.left;
            }
            
            if (!stack.isEmpty()) {
                cur = stack.pop();
            }
        }

        return res;
    }
}
```

### Simple method

```java
// 前序遍历顺序：中-左-右，入栈顺序：中-右-左
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null){
            return result;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()){
            TreeNode node = stack.pop();
            result.add(node.val);
            if (node.right != null){
                stack.push(node.right);
            }
            if (node.left != null){
                stack.push(node.left);
            }
        }
        return result;
    }
}
```

## Recursion

### V1 - not clean

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root != null ) res = traverse(root, res); // root can be null
        return res;
        
    }
    private List<Integer> traverse(TreeNode node, List<Integer> list) {
        // if (node != null)
        list.add(node.val);

        if (node.left != null) {
            list = traverse(node.left, list);
        }

        if (node.right != null) {
            list = traverse(node.right, list);
        }
        return list;
    }
}
```

### V2 - cleaner

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        traverse(root, res); // reference
        return res;
        
    }
    private void traverse(TreeNode node, List<Integer> list) {
        if (node == null) return;
        list.add(node.val);

        traverse(node.left, list);
        traverse(node.right, list);
    }
}
```

## Unified

### Null pointer mark method (middle node)

> push stack in reverse traversal order, and mark middle node
> 

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new LinkedList<>();
        Stack<TreeNode> st = new Stack<>();
        if (root != null) st.push(root);
        while (!st.empty()) {
            TreeNode node = st.peek();
            if (node != null) {
                st.pop(); // 将该节点弹出，避免重复操作，下面再将右左中节点添加到栈中（前序遍历-中左右，入栈顺序右左中）
                if (node.right!=null) st.push(node.right);  // 添加右节点（空节点不入栈）
                if (node.left!=null) st.push(node.left);    // 添加左节点（空节点不入栈）
                st.push(node);                          // 添加中节点
                st.push(null); // 中节点访问过，但是还没有处理，加入空节点做为标记。

            } else { // 只有遇到空节点的时候，才将下一个节点放进结果集
                st.pop();           // 将空节点弹出
                node = st.peek();    // 重新取出栈中元素
                st.pop();
                result.add(node.val); // 加入到结果集
            }
        }
        return result;
    }
}
```

### Boolean mark method

> Similar to null pointer mark method
> 

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new LinkedList<>();
        Stack<TreeNode> st = new Stack<>();
        if (root != null) st.push(root);
        while (!st.empty()) {
            TreeNode node = st.peek();
            if (node != null) {
                st.pop(); // 将该节点弹出，避免重复操作，下面再将右左中节点添加到栈中（前序遍历-中左右，入栈顺序右左中）
                if (node.right!=null) st.push(node.right);  // 添加右节点（空节点不入栈）
                if (node.left!=null) st.push(node.left);    // 添加左节点（空节点不入栈）
                st.push(node);                          // 添加中节点
                st.push(null); // 中节点访问过，但是还没有处理，加入空节点做为标记。

            } else { // 只有遇到空节点的时候，才将下一个节点放进结果集
                st.pop();           // 将空节点弹出
                node = st.peek();    // 重新取出栈中元素
                st.pop();
                result.add(node.val); // 加入到结果集
            }
        }
        return result;
    }
}
```