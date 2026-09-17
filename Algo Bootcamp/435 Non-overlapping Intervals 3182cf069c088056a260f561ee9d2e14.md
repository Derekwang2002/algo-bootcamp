# 435. Non-overlapping Intervals

Date: Mar 2
Level: Medium
Minutes: 15
Review Recommendation: ✅ Good job
Status: Done
Tags: Greedy
Time Complexity: O(nlogn)
Space Complexity: O(1)
URL: https://leetcode.com/problems/non-overlapping-intervals/description/
Carl's: https://programmercarl.com/0435.%E6%97%A0%E9%87%8D%E5%8F%A0%E5%8C%BA%E9%97%B4.html

<aside>
💡

Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return *the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping*.

**Note** that intervals which only touch at a point are **non-overlapping**. For example, `[1, 2]` and `[2, 3]` are non-overlapping.

</aside>

# Thought

- Greedy strategy:
    - Sort by $end_i$ , if $end_i$ same, by $start_i$
    - check if (last $end_i$ > current $start_i$)
        - if true: remove current
        - if false: renew last $end_i$, go on

# Solution

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> {
            if (a[1] == b[1]) return a[0] - b[0];
            return a[1] - b[1];
        });

        int count = 0;
        int lastEnd = intervals[0][0];
        
        for (int[] i : intervals) {
            if (i[0] < lastEnd) {
                count++;
            } else {
                lastEnd = i[1];
            }
        }

        return count;
    }
}
```