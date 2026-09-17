# 452. Minimum Number of Arrows to Burst Balloons

Date: Mar 2
Level: Medium
Minutes: 10
Review Recommendation: ✅ Good job
Status: Done
Tags: Greedy
Time Complexity: O(nlogn)
Space Complexity: O(1)
URL: https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/
Carl's: https://programmercarl.com/0452.%E7%94%A8%E6%9C%80%E5%B0%91%E6%95%B0%E9%87%8F%E7%9A%84%E7%AE%AD%E5%BC%95%E7%88%86%E6%B0%94%E7%90%83.html

<aside>
💡

There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array `points` where `points[i] = [xstart, xend]` denotes a balloon whose **horizontal diameter** stretches between `xstart` and `xend`. You do not know the exact y-coordinates of the balloons.

Arrows can be shot up **directly vertically** (in the positive y-direction) from different points along the x-axis. A balloon with `xstart` and `xend` is **burst** by an arrow shot at `x` if `xstart <= x <= xend`. There is **no limit** to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array `points`, return *the **minimum** number of arrows that must be shot to burst all balloons*.

**In short: minimum x that could be included in all interval**

</aside>

# Thought

- Greedy strategy:
    - Sort on $x_{end}$  ascendingly
    - Set `x` on the first $x_{end}$, traverse sorted points, if current point not include `x`, set a new x at current point’s $x_{end}$
        - Count the number of `x`s seted.

# Solution

```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        Arrays.sort(points, (a, b) -> {
            return a[1] - b[1];
        });

        int x = points[0][1];
        int count = 1;

        for (int[] p : points) {
            if (!(p[0] <= x && x <= p[1])) {
                x = p[1];
                count++;
            }
        }

        return count;
    }
}
```