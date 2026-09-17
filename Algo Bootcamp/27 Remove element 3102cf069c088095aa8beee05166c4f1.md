# 27. Remove element

Date: Jan 19
Minutes: 15
Review Recommendation: ✅ Good job
Status: Done
Tags: Array, DoublePointer

# Thought

- 限制：不创建新数组(若创建则空间复杂度为`O(n)`)，不通过语法直接删除(删除函数终复杂度为`O(n)`).
- 思想：双指针  `(read, write)` or `(fast, slow)`, 一个判断，一个赋值
- 26，283，844，977

# Solution

```python
class Solution(object):
    def removeElement(self, nums, val):
        """
        :type nums: List[int]
        :type val: int
        :rtype: int
        """
        
        k = 0

        for n in nums:
            if n != val:
                nums[k] = n
                k += 1
            
        return k
```