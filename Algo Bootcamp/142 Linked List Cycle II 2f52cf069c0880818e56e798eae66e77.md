# 142. Linked List Cycle II

Date: Jan 27
Level: Hard
Minutes: 90
Review Recommendation: 🚩 Recommend Redo (Hard >50m)
Status: Done
Tags: DoublePointer, LinkedList

<aside>
💡

Given the `head` of a linked list, return *the node where the cycle begins. If there is no cycle, return* `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

</aside>

- Timer:
    - ***10min***  Find circle
    - ***10min***  Violent find entrance
    - ***1h+***  O(1) memory find entrance

# 思路：

- 快慢指针寻环，如果有环，则在某处相遇：`fast == slow`
    - 此时，`slow`至多走完$1$圈（`entrance`为头节点时），设fast走完$n(n≥1)$圈
    - **相遇点非常重要！思考这个点意味着什么**
- 将`slow`的路径表示为：$x+y$
    - 头节点至`entrance`为$x$，entrance至相遇点为$y$
- `fast`的路径可以表示为：$x+y+n(y+z)$
    - 环长等于$z + y$
- 考虑到`fast`的路径始终是`slow`的两倍，有如下数学关系
    
    $$
    x+y+n(y+z) = 2(x+y)
    $$
    
- 求x，可化简得：
    
    $$
    x=(n-1)(y+z)+z
    $$
    
- 注意到，从相遇点走 $(n-1)$ 圈再走 $z$ 步为`entrance`，其步数与从头节点至entrance的步数相等。所以，从头节点和相遇点分别出发，相遇时的节点即为`entrance`
- 评价：
    - 判断环存在较简单，但是通过数学关系寻找`entrance`的难度较大
    - 不过暴力寻找`entrance`可以通过

# Solution

## My solution

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """

        # ========= Tail alignment ========
        if headA == None or headB == None: return None

        cur_a, cur_b = headA, headB # not None
        count_a, count_b = 1, 1

        # cur point to the last one
        while cur_a.next: 
            cur_a = cur_a.next
            count_a += 1

        while cur_b.next: 
            cur_b = cur_b.next
            count_b += 1

        # gurantee must have intersection
        if cur_a != cur_b: return None

        # point back
        cur_a, cur_b = headA, headB
        
        gap = abs(count_a - count_b)
        longer = max(count_a, count_b)
        
        # alignment
        if longer == count_a:
            for _ in range(gap):
                cur_a = cur_a.next
        else:
            for _ in range(gap):
                cur_b = cur_b.next

        # loop till find the intersection (must can find)
        while cur_a != cur_b:
            cur_a = cur_a.next
            cur_b = cur_b.next

        return cur_a
```

## Carl’s

```python
（版本三）求长度，同时出发 （代码复用 + 精简）
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        dis = self.getLength(headA) - self.getLength(headB)

        # 通过移动较长的链表，使两链表长度相等
        if dis > 0:
            headA = self.moveForward(headA, dis)
        else:
            headB = self.moveForward(headB, abs(dis))

        # 将两个头向前移动，直到它们相交
        while headA and headB:
            if headA == headB:
                return headA
            headA = headA.next
            headB = headB.next

        return None

    def getLength(self, head: ListNode) -> int:
        length = 0
        while head:
            length += 1
            head = head.next
        return length

    def moveForward(self, head: ListNode, steps: int) -> ListNode:
        while steps > 0:
            head = head.next
            steps -= 1
        return head
```