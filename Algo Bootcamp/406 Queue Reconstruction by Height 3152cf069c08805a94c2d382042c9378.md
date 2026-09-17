# 406. Queue Reconstruction by Height

Date: Feb 28
Level: Medium
Minutes: 50
Review Recommendation: 🚩 Recommend Redo (Medium >40m)
Status: Review-logic
Tags: Greedy
Time Complexity: O(n^2)
Space Complexity: O(n)
URL: https://leetcode.com/problems/queue-reconstruction-by-height/description/
Carl's: https://programmercarl.com/0406.%E6%A0%B9%E6%8D%AE%E8%BA%AB%E9%AB%98%E9%87%8D%E5%BB%BA%E9%98%9F%E5%88%97.html

<aside>
💡

You are given an array of people, `people`, which are the attributes of some people in a queue (not necessarily in order). Each `people[i] = [hi, ki]` represents the `ith` person of height `hi` with **exactly** `ki` other people in front who have a height greater than or equal to `hi`.

Reconstruct and return *the queue that is represented by the input array* `people`. The returned queue should be formatted as an array `queue`, where `queue[j] = [hj, kj]` is the attributes of the `jth` person in the queue (`queue[0]` is the person at the front of the queue).

</aside>

# Thought

> **Failed** to come up a greedy strategy!
> 
- Greedy strategy:
    - First sort based on height(desc), if height equals, k(asce)
    - Then insert each person to the queue based on this order. Why?
        - the later inserted person has lower height, won’t affect previous people.
        - if same height, lower k first, so later person won’t affect previous person(higher k always at behind)
- Time complexity:
    - sort: O(nlogn)
    - insert: O(n^2) (n times loop, linked list insertion need O(n))

# Solution

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        // 身高从大到小排（身高相同k小的站前面）
        Arrays.sort(people, (a, b) -> {
            if (a[0] == b[0]) return a[1] - b[1]; // ascending
            return b[0] - a[0]; // descending
        });

        LinkedList<int[]> que = new LinkedList<>();

        for (int[] p : people) {
            que.add(p[1],p);   //Linkedlist.add(index, value)，put value @ index
        }

        return que.toArray(new int[people.length][]);
    }
}
```