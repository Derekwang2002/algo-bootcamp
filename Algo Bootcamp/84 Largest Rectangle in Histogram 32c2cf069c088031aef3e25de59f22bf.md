# 84. Largest Rectangle in Histogram

Date: Mar 22
Level: Hard
Minutes: 120
Review Recommendation: 🚩 Recommend Redo (Hard >50m)
Status: Done
Tags: DoublePointer, MonotonicStack
Time Complexity: O(n)
Space Complexity: O(n)
URL: https://leetcode.com/problems/largest-rectangle-in-histogram/description/
Carl's: https://programmercarl.com/0084.%E6%9F%B1%E7%8A%B6%E5%9B%BE%E4%B8%AD%E6%9C%80%E5%A4%A7%E7%9A%84%E7%9F%A9%E5%BD%A2.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return *the area of the largest rectangle in the histogram*.

</aside>

# Thought

- Idea:
    - for each `i`, store rectangle area get from **moving `height[i]`**,
    - return max area.
    - (suppose stored in `int[] area`)
- Implement (for heights[i]):
    - find *first-samller* height(index) on the left(`l`)&right(`r`)
        - **when an item popped, traversing item is r, stack top is l.**
        - if no, then let `l = -1`, `r = heights.length`
    - $area[i] = areaOnRight + areaOnLeft + heights[i]$
        - $areaOnLeft = heights[i] \times (i - l - 1)$
        - $areaOnRight = heights[i] \times (r - i - 1)$
- Trick to clean code:
    - Problem: when finish traversing, there might be some items left in mono stack
    - Solve: **add sentinel 0** before head & after end of heights
    - Useful method: **System.arraycopy()**
        - `System.arraycopy(heights, 0, newHeight, 1, heights.length);`
            
            
            | parameters | meaning | in example |
            | --- | --- | --- |
            | **`src`** | source array | `heights` |
            | **`srcPos`** | source array start index | `0` |
            | **`dest`** | target array | `newHeight` |
            | **`destPos`** | target array start index | `1` |
            | **`length`** | copy length | `heights.length` |

# Solution

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int[] rec = new int[heights.length];
        Deque<Integer> mono = new ArrayDeque<>();
        
        for (int i = 0; i < heights.length; i++) {
            rec[i] += heights[i];

            while (!mono.isEmpty() && heights[i] < heights[mono.peek()]) {
                int popIdx = mono.pop();
                rec[popIdx] += heights[popIdx] * (i - popIdx - 1);
            }

            mono.push(i);
        }

        while (!mono.isEmpty()) {
            int popIdx = mono.pop();
            rec[popIdx] += heights[popIdx] * (heights.length - 1 - popIdx);
        }

        for (int i = heights.length - 1; i >= 0 ; i--) {
            while (!mono.isEmpty() && heights[i] < heights[mono.peek()]) {
                int popIdx = mono.pop();
                rec[popIdx] += heights[popIdx] * (popIdx - i - 1);
            }

            mono.push(i);
        }

        while (!mono.isEmpty()) {
            int popIdx = mono.pop();
            rec[popIdx] += heights[popIdx] * (popIdx - 0);
        }

        int res = heights[0];
        for (int s : rec) res = Math.max(res, s);

        return res;
    }
}
```

### Optimized

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int[] newHeight = new int[heights.length + 2];
        System.arraycopy(heights, 0, newHeight, 1, heights.length);
        
        newHeight[heights.length+1] = 0;
        newHeight[0] = 0;

        Deque<Integer> stack = new ArrayDeque<>();
        stack.push(0);

        int res = 0;
        for (int i = 1; i < newHeight.length; i++) {
            while (newHeight[i] < newHeight[stack.peek()]) {
            
                int mid = stack.pop();
                int width = i - stack.peek() - 1;
                int height = newHeight[mid];
                
                res = Math.max(res, w * h);
            }
            
            stack.push(i);
        }
        
        return res;
    }
}
```