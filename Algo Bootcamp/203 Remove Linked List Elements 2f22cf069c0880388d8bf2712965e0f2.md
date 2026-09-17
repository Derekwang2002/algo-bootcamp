# 203. Remove Linked List Elements

Date: Jan 24
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: LinkedList

<aside>
💡

Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that has `Node.val == val`, and return *the new head*.

</aside>

# Thought

- 链表基础题，遍历判断并删除即可
    - **注意**！- 1.while循环的条件；2.虚拟头节点时的返回值（`dummy_head.next`）
- 问题：头节点的判断逻辑和后面节点的判断逻辑不一致，需要单独处理
    - 解决：设置虚拟头节点 `dummy node`，最后返回 `dummy node → next`
- 递归思路：
    - 中止case：空链表直接返回
    - 递归case：检查头节点，
        - 如果`==val`则返回之后链表的结果，
        - 如果`≠val`则返回头节点与之后链表结果的拼接
    - 递归C++代码：
        
        ```python
        class Solution {
        public:
            ListNode* removeElements(ListNode* head, int val) {
                // 基础情况：空链表
                if (head == nullptr) {
                    return nullptr;
                }
        
                // 递归处理
                if (head->val == val) {
                    ListNode* newHead = removeElements(head->next, val);
                    delete head;
                    return newHead;
                } else {
                    head->next = removeElements(head->next, val);
                    return head;
                }
            }
        };
        ```
        

# Solution

## Mine

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def removeElements(self, head, val):
        """
        :type head: Optional[ListNode]
        :type val: int
        :rtype: Optional[ListNode]
        """

        while head != None and head.val == val:
            head = head.next

        cur = head

        while cur!= None and cur.next != None:
            if cur.next.val == val:
                cur.next = cur.next.next
            else:
                cur = cur.next

        return head

```

## Carl’s

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # 创建虚拟头部节点以简化删除过程
        dummy_head = ListNode(next = head)
        
        # 遍历列表并删除值为val的节点
        current = dummy_head
        while current.next:
            if current.next.val == val:
                current.next = current.next.next
            else:
                current = current.next
        
        return dummy_head.next
```