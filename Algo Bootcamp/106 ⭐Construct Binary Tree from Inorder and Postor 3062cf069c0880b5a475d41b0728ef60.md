# 106. ⭐Construct Binary Tree from Inorder and Postorder Traversal

Date: Feb 12
Minutes: 60
Review Recommendation: ✅ Good job
Status: Done
Tags: BinaryTree

# Thought:

- Divide and conquer - recursion
    - Observation (each step in recursion):
        - For postorder list, the last value is the root value of the current tree. New a TreeNode `root` and store the value.
        - Since the value of the tree is unique, we can find the root element in inorder list
        - For inorder list: the left/right part of the root element is corresponding left/right subtree, we can get length of both by whole list `len` and inorder’s `mid` index
        - We can the subtract left/right subtree element from postorder list, former part is left subtree, and rear part is right subtree.
        - Now we got 4 sub list for left/right and in/post-order, then **find root of left/right subtree** and asign them to `root` node.
        - Return `root` at last, this forms a recursion.
    - Recursion function:
        - return type `TreeNode`.
        - base case: 1.`len==0` → return null; 2.`len==1` → return root.
- Evaluation:
    - Thought is easy, but efficiency need consider.
    - **Improvement**:
        - Use hashmap to store inorder (value-index), O(1) get root index;
        - To avoid creating new array at each step, use virtual cutting by begin&end indexes[Begin, End), which need a helper function to store
    - Optimal Implemetation:
        - Set postorder list as global variable, use single pointer(also a global variable) move left to right.
        - Recursive cut inorder list `(int[] inorder, int inL, int inR)` which is [inL, inR].
        - Traverse in the **reverse order of a postorder** traversal(because we need to follow the pointer)
    - Traverses order matters!

# Solution

## Raw

```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        int len = postorder.length;
        if (len == 0) {
            return null;
        }

        TreeNode root = new TreeNode(postorder[len - 1]);
        if (len == 1) {
            return root;
        }

        int mid = 0;
        for (int i = 0; i < len; i++) {
            if (inorder[i] == root.val) {
                mid = i;
            }
        }

        int rightLen = len - mid - 1;
        int leftLen = mid;

        int[] rightIn = new int[rightLen];
        for (int i = 0; i < rightLen; i++) {
            rightIn[i] = inorder[i + mid + 1];
        }

        int[] leftIn = new int[leftLen];
        for (int i = 0; i < leftLen; i++) {
            leftIn[i] = inorder[i];
        }

        int[] rightPost = new int[rightLen];
        for (int i = 0; i < rightLen; i++) {
            rightPost[i] = postorder[leftLen + i];
        }
 
        int[] leftPost = new int[leftLen];
        for (int i = 0; i < leftLen; i++) {
            leftPost[i] = postorder[i];
        }

        root.left = buildTree(leftIn, leftPost);
        root.right = buildTree(rightIn, rightPost);
        return root;
    }
}
```

## Better

```java
class Solution {
    Map<Integer, Integer> map;  // 方便根据数值查找位置
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        map = new HashMap<>();
        for (int i = 0; i < inorder.length; i++) { // 用map保存中序序列的数值对应位置
            map.put(inorder[i], i);
        }

				// 前闭后开
        return findNode(inorder, 0, inorder.length, postorder, 0, postorder.length);  
    }

    public TreeNode findNode(int[] inorder, int inBegin, int inEnd, int[] postorder, int postBegin, int postEnd) {
        // 参数里的范围都是前闭后开
        if (inBegin >= inEnd || postBegin >= postEnd) {  
            return null; // 不满足左闭右开，说明没有元素，返回空树
        }
        
        // 找到后序遍历的最后一个元素在中序遍历中的位置
        int rootIndex = map.get(postorder[postEnd - 1]);  
        TreeNode root = new TreeNode(inorder[rootIndex]);  // 构造结点
        int lenOfLeft = rootIndex - inBegin;  // 保存中序左子树个数，用来确定后序数列的个数
        
        root.left = findNode(inorder, inBegin, rootIndex,
                            postorder, postBegin, postBegin + lenOfLeft);
        root.right = findNode(inorder, rootIndex + 1, inEnd,
                            postorder, postBegin + lenOfLeft, postEnd - 1);

        return root;
    }
}
```

## Optimal

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    private int[] postorder;
    private Map<Integer, Integer> inPos;
    private int postIdx;

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        int n = inorder.length;

        this.postorder = postorder;
        this.postIdx = n - 1;

        inPos = new HashMap<>();
        for (int i = 0; i < n; i++) {
            inPos.put(inorder[i], i);
        }

        return build(0, n - 1);
    }

    private TreeNode build(int inL, int inR) {
        if (inL > inR) return null;

        int rootVal = postorder[postIdx--];
        TreeNode root = new TreeNode(rootVal);

        int mid = inPos.get(rootVal);

        // 注意顺序：必须先右后左
        root.right = build(mid + 1, inR);
        root.left  = build(inL, mid - 1);

        return root;
    }
}

```