# 19. Remove Nth Node From End of List

Date: Jan 26
Minutes: 25
Review Recommendation: ✅ Good job
Status: Done
Tags: DoublePointer, LinkedList

<aside>
💡

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

</aside>

# 思路：

- 双指针（用到三个）
    - 使用`cur`指针遍历链表，同时根据`cur`的位置，设置一个`cur`之后n个元素的`tail`指针，这样在`tail`指针走到链表尾时，cur指针就是倒数第n个需要删除的节点
    - 再使用`prev`指针(初始化为dummy_head)表示指向要删除的指针(`cur`)的指针，负责删除后更新
    - 由于函数要求返回`head`，所以在遍历操作中，不对其做修改，最后直接返回。但是会出现一种特殊情况：当删除的节点是`head`时，函数则需要返回`head.next`(`cur.next`)
        - 判断：`cur == head`
- 评价：操作简单，要注意特殊/边界情况的判定
    - Carl’s solution: 快慢指针，全部初始化至`dummy_head`，无特殊情况，返回 `dummy_head.next`, 更优

# Solution

## My solution

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: Optional[ListNode]
        :type n: int
        :rtype: Optional[ListNode]
        """
        prev = ListNode(next=head)
        cur = head

        while True:
            tail = cur

            for _ in range(n):
                if (tail): tail = tail.next
                else: return head

            if tail == None:
                # delete cur
                prev.next = cur.next
                if (cur == head): head = cur.next
                return head
            
            prev = cur
            cur = cur.next
```

## Carl’s

```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        # 创建一个虚拟节点，并将其下一个指针设置为链表的头部
        dummy_head = ListNode(0, head)

        # 创建两个指针，慢指针和快指针，并将它们初始化为虚拟节点
        slow = fast = dummy_head

        # 快指针比慢指针快 n+1 步
        for i in range(n+1):
            fast = fast.next

        # 移动两个指针，直到快速指针到达链表的末尾
        while fast:
            slow = slow.next
            fast = fast.next

        # 通过更新第 (n-1) 个节点的 next 指针删除第 n 个节点
        slow.next = slow.next.next

        return dummy_head.next
```