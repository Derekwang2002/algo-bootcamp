# 94. Binary Tree Inorder Traversal

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

### V1 - mess code

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;

        Deque<TreeNode> stack = new ArrayDeque<>();
        Set<TreeNode> set = new HashSet<>();
        TreeNode cur = root;

        stack.push(root);

        while (!stack.isEmpty() || !set.contains(root)) {
            if (cur == null || set.contains(cur)) {
                cur = stack.pop();
                set.add(cur);
                res.add(cur.val);

                if (cur.right != null) {
                    stack.push(cur.right);
                }

                cur = stack.peek();
                continue;
            }

            if (stack.peek() != cur) {
                stack.push(cur);
            }

            cur = cur.left;
        }

        return res;
    }
}
```

### V2 - cleaner code

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;

        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode cur = root;

        while (cur != null || !stack.isEmpty()){
           if (cur != null){
               stack.push(cur);
               cur = cur.left;
           } else {
               cur = stack.pop();
               res.add(cur.val);
               cur = cur.right;
           }
        }

        return res;
    }
}
```

## Recursion

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        traverse(root, res);
        return res;
    }

    private void traverse(TreeNode node, List<Integer> list) {
        if (node == null) {
            return;
        }

        traverse(node.left, list);
        list.add(node.val);
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
public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new LinkedList<>();
    Stack<TreeNode> st = new Stack<>();
    if (root != null) st.push(root);
    while (!st.empty()) {
        TreeNode node = st.peek();
        if (node != null) {
            st.pop(); // 将该节点弹出，避免重复操作，下面再将右中左节点添加到栈中（中序遍历-左中右，入栈顺序右中左）
            if (node.right!=null) st.push(node.right);  // 添加右节点（空节点不入栈）
            st.push(node);                          // 添加中节点
            st.push(null); // 中节点访问过，但是还没有处理，加入空节点做为标记。
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
}
```

### Boolean mark method

> Similar to null pointer mark method
> 

```python
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        values = []
        stack = [(root, False)] if root else [] # 多加一个参数，False 为默认值，含义见下文

        while stack:
            node, visited = stack.pop() # 多加一个 visited 参数，使“迭代统一写法”成为一件简单的事

            if visited: # visited 为 True，表示该节点和两个儿子的位次之前已经安排过了，现在可以收割节点了
                values.append(node.val)
                continue

            # visited 当前为 False, 表示初次访问本节点，此次访问的目的是“把自己和两个儿子在栈中安排好位次”。
            # 中序遍历是'左中右'，右儿子最先入栈，最后出栈。
            if node.right:
                stack.append((node.right, False))

            stack.append((node, True)) # 把自己加回到栈中，位置居中。同时，设置 visited 为 True，表示下次再访问本节点时，允许收割

            if node.left:
                stack.append((node.left, False)) # 左儿子最后入栈，最先出栈

        return values
```