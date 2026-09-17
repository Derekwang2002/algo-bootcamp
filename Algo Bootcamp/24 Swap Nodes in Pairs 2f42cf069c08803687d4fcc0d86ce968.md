# 24. Swap Nodes in Pairs

Date: Jan 25
Minutes: 35
Review Recommendation: ✅ Good job
Status: Done
Tags: LinkedList

<aside>
💡

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

</aside>

# 思路：

- 以step=2遍历链表（while循环），只有一个元素时单独判断
- 交换相邻元素时，需要将`cur`指针指向前一位元素
    - 先 根据`cur` 保存相邻元素至`front`，`rear`（交换前）
        - 在第一步将结果变量`res`指向`rear`用来返回
    - 更新目前`cur.next`指针至`rear`（交换后的前者）
    - 更新`cur`指针至`front`（交换后的后者）
    - 交换：将`front.next`指向`rear.next`；将`rear.next`指向`front`
- 评价：操作略微复杂但思路简单易懂
    - 单链表中的指针维护
    - 不借助helper function完成递归

# Solution

## My solution

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def swapPairs(self, head):
        """
        :type head: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        cur = ListNode(next=head) # dummy_head
        res = None

				# case for single element
        if (head != None and head.next == None):
            return head

        while (cur.next != None and cur.next.next != None):
		        # get swapping pair
            front = cur.next
            rear = front.next

            if (front == head):
                res = rear
                
						# maintain former pointer
            cur.next = rear
            cur = front # update

						# swapped
            front.next = rear.next
            rear.next = front

        return res
```

## Carl’s

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        dummy_head = ListNode(next=head)
        current = dummy_head

        # 必须有cur的下一个和下下个才能交换，否则说明已经交换结束了
        while current.next and current.next.next:
            temp = current.next # 防止节点修改
            temp1 = current.next.next.next

            current.next = current.next.next
            current.next.next = temp
            temp.next = temp1
            current = current.next.next
        return dummy_head.next
```

```python
# 递归版本
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head

        # 待翻转的两个node分别是pre和cur
        pre = head
        cur = head.next
        next = head.next.next

        cur.next = pre  # 交换
        pre.next = self.swapPairs(next) # 将以next为head的后续链表两两交换

        return cur
```