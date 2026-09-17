# 42. Trapping Rain Water

Date: Mar 21
Level: Hard
Minutes: 120
Review Recommendation: 🚩 Recommend Redo (Hard >50m)
Status: Review-logic
Tags: DoublePointer, MonotonicStack
Time Complexity: O(n)
Space Complexity: O(n)
URL: https://programmercarl.com/0042.%E6%8E%A5%E9%9B%A8%E6%B0%B4.html
Carl's: https://leetcode.com/problems/trapping-rain-water/description/

<aside>
💡

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

</aside>

# Thought

> **FAILED (but close)**
> 
- Amount of water for a point:
    
    $$
     \text{water}[i] = \min(\text{max\_left}, \text{max\_right}) - \text{height}[i]
    $$
    

### Monotonic stack

- *Thoutht*: filling water **horizontally**
    - Logic: in mono stack, find first greater at right, left item must greater as well, then a horizontal space of water between left&right can be determined
- Implementation:
    - **traverse height array**, if an item popped → a “valley” is found
        - floor: popped item, `stack.pop()`
        - right wall: current height, `i`
        - left wall: height at `stack.peek()` (if stack not empty)
    - amount calculation(for ‘a pop’): $distance \times waterH$
        - $distance = i(currentIndex) - stack.peek() - 1$
        - $waterH = min\{height[leftWall], height[rightwall]\} - height[floor]$

### Double pointer

- *Thoutht*: filling water **vertically**
    - Logic: trap water first at place that has lower edge, and skip place that are the highest in some direction.
- **Key trick: record max of left/right in efficient way**
    - Solution: move double pointer from left & right to mid
- Implementation(`=` can at either side):
    - `height[l] < height[r]` → for *l*, maxLeft < maxRight, so it can trap water: **$maxLeft - height[l]$**
        - If height[l] ≥ maxLeft, renew maxLeft and can’t trap water
    - `height[l] >= height[r]` → for *r*, maxLeft ≥ maxRight, so it can trap water: $maxRight - height[r]$
        - If height[r] ≥ maxRight, renew maxRight and can’t trap water

# Solution

### Monotonic stack

```java
class Solution {
    public int trap(int[] height) {
        Deque<Integer> mono = new ArrayDeque<>();
        int water = 0;

        for (int i = 0; i < height.length; i++) {
            while (!mono.isEmpty() && height[i] > height[mono.peek()]) {
                int index = mono.pop();

                if (mono.isEmpty()) break;
                int leftIdx = mono.peek();
                
                int dist = i - leftIdx - 1;
                int waterH = Math.min(height[i], height[leftIdx]) - height[index];
                water += waterH * dist;
            }

            mono.push(i);
        }

        return water;
    }
}
```

### Double pointer (Faster)

- O(1) space

```java
// optimized
class Solution {
    public int trap(int[] height) {
        int l = 0, r = height.length - 1;
        int maxLeft = 0, maxRight = 0;
        int res = 0;

        while (l < r) {
            if (height[l] < height[r]) {
                // If current height is a new max, update it; 
                // otherwise, the difference is the trapped water.
                if (height[l] >= maxLeft) maxLeft = height[l];
                else res += maxLeft - height[l];
                l++;
            } else {
                if (height[r] >= maxRight) maxRight = height[r];
                else res += maxRight - height[r];
                r--;
            }
        }
        return res;
    }
}
```

```java
// carl's
class Solution {
    public int trap(int[] height) {
        if (height.length <= 2) {
            return 0;
        }
        // 从两边向中间寻找最值
        int maxLeft = height[0], maxRight = height[height.length - 1];
        int l = 1, r = height.length - 2;
        int res = 0;
        while (l <= r) {
            // 不确定上一轮是左边移动还是右边移动，所以两边都需更新最值
            maxLeft = Math.max(maxLeft, height[l]);
            maxRight = Math.max(maxRight, height[r]);
            // 最值较小的一边所能装的水量已定，所以移动较小的一边。
            if (maxLeft < maxRight) {
                res += maxLeft - height[l ++];
            } else {
                res += maxRight - height[r --];
            }
        }
        return res;
    }
}
```

```java
// easier to understand
class Solution {
    public int trap(int[] height) {
        int length = height.length;
        if (length <= 2) return 0;
        int[] maxLeft = new int[length];
        int[] maxRight = new int[length];

        // 记录每个柱子左边柱子最大高度
        maxLeft[0] = height[0];
        for (int i = 1; i< length; i++) maxLeft[i] = Math.max(height[i], maxLeft[i-1]);

        // 记录每个柱子右边柱子最大高度
        maxRight[length - 1] = height[length - 1];
        for(int i = length - 2; i >= 0; i--) maxRight[i] = Math.max(height[i], maxRight[i+1]);

        // 求和
        int sum = 0;
        for (int i = 0; i < length; i++) {
            int count = Math.min(maxLeft[i], maxRight[i]) - height[i];
            if (count > 0) sum += count;
        }
        return sum;
    }
}
```