# 145. Binary Tree Postorder Traversal

Date: Feb 9
Minutes: 65
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

# Thought

- Recursive: Use one helper function, take a list to store result, and control order
- Iterative: Asynchronous visit node and add result (maintain stack)

# Solution

## Iteration

### V1 - Bad time complexity

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }

        Deque<TreeNode> stack = new ArrayDeque<>();
        Map<TreeNode, Integer> map = new HashMap<>();
        TreeNode cur = root;        

        while (cur != null || !stack.isEmpty()) {
            if (cur != null) {
                if (map.getOrDefault(cur, 0) == 2){ // outlet
                    res.add(stack.pop().val);
                    cur = stack.peek();
                } else if (map.getOrDefault(cur, 0) == 1) { 
                    // finish left subtree
                    map.put(cur, 2);
                    cur = cur.right;
                } else { // first met node
                    map.put(cur, 1);
                    stack.push(cur);
                    cur = cur.left;
                }
            } else { // cur == null
                cur = stack.peek();
            }
        }

        return res;
    }
}
```

### cur-prev

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode cur = root, prev = null;

        while (cur != null || !stack.isEmpty()) {
            // 1) 一路向左入栈
            while (cur != null) {
                stack.push(cur);
                cur = cur.left;
            }

            // 2) 看栈顶（不弹出）
            TreeNode node = stack.peek();

            // 3) 如果右子树不存在，或右子树已经访问过，则可以访问当前节点
            if (node.right == null || node.right == prev) {
                res.add(node.val);
                stack.pop();
                prev = node;     // 标记刚访问的节点
            } else {
                // 4) 否则先处理右子树
                cur = node.right;
            }
        }

        return res;
    }
}
```

### Preorder reverse

```java
// 后序遍历顺序 左-右-中 入栈顺序：中-左-右 出栈顺序：中-右-左， 最后翻转结果
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null){
            return result;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()){
            TreeNode node = stack.pop();
            result.add(node.val);
            if (node.left != null){
                stack.push(node.left);
            }
            if (node.right != null){
                stack.push(node.right);
            }
        }
        Collections.reverse(result);
        return result;
    }
}
```

## Recursion

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        traverse(root, res);
        return res;
    }
    private void traverse(TreeNode node, List<Integer> list) {
        if (node == null) {
            return;
        }

        traverse(node.left, list);
        traverse(node.right, list);
        list.add(node.val);
    }
}
```

## Unified

### Null pointer mark method (middle node)

> push stack in reverse traversal order, and mark middle node
> 

```java
class Solution {
   public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new LinkedList<>();
        Stack<TreeNode> st = new Stack<>();
        if (root != null) st.push(root);
        while (!st.empty()) {
            TreeNode node = st.peek();
            if (node != null) {
                st.pop(); // 将该节点弹出，避免重复操作，下面再将中右左节点添加到栈中（后序遍历-左右中，入栈顺序中右左）
                st.push(node);                          // 添加中节点
                st.push(null); // 中节点访问过，但是还没有处理，加入空节点做为标记。
                if (node.right!=null) st.push(node.right);  // 添加右节点（空节点不入栈）
                if (node.left!=null) st.push(node.left);    // 添加左节点（空节点不入栈）         
                               
            } else { // 只有遇到空节点的时候，才将下一个节点放进结果集
                st.pop();           // 将空节点弹出
                node = st.peek();    // 重新取出栈中元素
                st.pop();
                result.add(node.val); // 加入到结果集
            }
        }
        return result;
   }
```

### Boolean mark method

> Similar to null pointer mark method
> 

```python
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        values = []
        stack = [(root, False)] if root else [] # 多加一个参数，False 为默认值，含义见下文

        while stack:
            node, visited = stack.pop() # 多加一个 visited 参数，使“迭代统一写法”成为一件简单的事

            if visited: # visited 为 True，表示该节点和两个儿子位次之前已经安排过了，现在可以收割节点了
                values.append(node.val)
                continue

            # visited 当前为 False, 表示初次访问本节点，此次访问的目的是“把自己和两个儿子在栈中安排好位次”
            # 后序遍历是'左右中'，节点自己最先入栈，最后出栈。
            # 同时，设置 visited 为 True，表示下次再访问本节点时，允许收割。
            stack.append((node, True))

            if node.right:
                stack.append((node.right, False)) # 右儿子位置居中

            if node.left:
                stack.append((node.left, False)) # 左儿子最后入栈，最先出栈

        return values
```