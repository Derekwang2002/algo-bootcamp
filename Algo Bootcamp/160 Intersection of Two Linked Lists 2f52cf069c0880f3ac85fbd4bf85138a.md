# 160. Intersection of Two Linked Lists

Date: Jan 26
Minutes: 60
Review Recommendation: ✅ Good job
Status: Done
Tags: DoublePointer, LinkedList

<aside>
💡

Given the heads of two singly linked-lists `headA` and `headB`, return *the node at which the two lists intersect*. If the two linked lists have no intersection at all, return `null`.

</aside>

- Timer:
    - ***10min*** Think + search syntax (zip)
    - ***15min*** Violent solution (Exceed Time Limit)
    - ***35min*** Tail alignment solution (passed)

# 思路：

- 排除任何一个是None的情况（非必要）
- 计算各自长度（关键），并比较最后一位是否是同一节点（非必要）
- 使 较长的链表的cur指针 指向 与较短链表 距离链表尾相同距离的元素（尾对齐）
- 将各自cur指针向后移动，当两链表cur指针相等时，返回所指向的node
    - 直到cur指针指向None，若没有则返回None
- 评价：
    - Easy题，但是不简单，暴力解 $O(m*n)$ 会超时
    - 无特殊情况，单独if判断非必须
    - 有代码复用
    - 特殊解法：等比例法
        - 排除None链表情况，同步交叉循环，直到找到交点（None表示无实际交点）
        - Code
            
            ```python
              def getIntersectionNode(self, headA: ListNode, headB: ListNode):
                  # 处理边缘情况
                  if not headA or not headB:
                      return None
            
                  # 在每个链表的头部初始化两个指针
                  pointerA = headA
                  pointerB = headB
            
                  # 遍历两个链表直到指针相交
                  while pointerA != pointerB:
                      # 将指针向前移动一个节点
                      pointerA = pointerA.next if pointerA else headB
                      pointerB = pointerB.next if pointerB else headA
            
                  # 如果相交，指针将位于交点节点，如果没有交点，值为None
                  return pointerA
            ```
            

# Solution

## My solutions

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution(object):
    def detectCycle(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        
        # double pointer find circle + violent find entrance
        
        slow = fast = head
        entrance = None
        circle = False

				# if exist circle
        while slow and fast and fast.next:
            slow = slow.next
            fast = fast.next.next
          
            if slow == fast: # circle exist
                circle = True
                break

				# find entrance if has
        if circle:
            past_node = []
            cur = head
            while cur not in past_node:
                past_node.append(cur)
                cur = cur.next
            entrance = cur

        return entrance
```

```python
  def detectCycle(self, head):
      """
      :type head: ListNode
      :rtype: ListNode
      """
      
      slow = fast = head

      while slow and fast and fast.next:
          slow = slow.next
          fast = fast.next.next
        
          if slow == fast: # circle exist
              slow = head 

              while slow != fast:
                  slow = slow.next
                  fast = fast.next

              return slow

      return None
```

## Carl’s solution (set method)

```python
（版本二）集合法
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        visited = set()

        while head:
            if head in visited:
                return head
            visited.add(head)
            head = head.next

        return None
```