# 206. Reverse Linked List

Date: Jan 25
Minutes: 40
Review Recommendation: ✅ Good job
Status: Done
Tags: DoublePointer, LinkedList

<aside>
💡

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

</aside>

# 思路：

- 从`head`开始遍历链表，`cur`指针表示当前元素
- 每轮中，先保存`cur.next`指向的元素，再让`cur.next`指针指向`cur`的前一位
    - `cur.next`指向的元素用`head`维护
    - 前一位就是目前已经反转的链表头，用`reversed_list`表示（初始化为`None`）
- 现在`cur`完成反转操作，更新`reversed_list`，指向`cur`
- 更新`cur`前进，令`cur`指向`head`（下一个）
- 评价：经典题目，有难度，要求思路清晰
    - 边界处理（通常是头元素）
    - 指针交替，明确需要维护的数据

# Solutions

### Iteratively

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def reverseList(self, head):
        """
        :type head: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        # iteratively
        cur = head
        reversed_list = None
        
        while (cur != None):

            head = cur.next # let head refers to the rest of the unreversed list
            cur.next = reversed_list # break link
            reversed_list = cur

            cur = head

        return reversed_list
```

### Recursively

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def reverseList(self, head):
        """
        :type head: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        # recursively
        def reverseHelper(reversed_list, cur):
            if cur == None: return reversed_list

            head = cur.next
            cur.next = reversed_list
            reversed_list = cur
            cur = head

            return reverseHelper(reversed_list, cur)

        return reverseHelper(None, head)
```

> **Carl’s solution is almsot same, no copy here**
>