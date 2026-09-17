# 349. Intersection of Two Arrays

Date: Jan 27
Minutes: 20
Review Recommendation: ✅ Good job
Status: Done
Tags: HashTable

<aside>
💡

Given two integer arrays `nums1` and `nums2`, return *an array of their intersection*. Each element in the result must be **unique** and you may return the result in **any order**.

</aside>

# 思路：

- list：
    - 要求题目限制了数值的大小
    - 要求数值分散均匀，如果哈希值比较少、特别分散，使用数组会造成空间的极大浪费
- Set：
    - 占用空间比数组大
    - 速度比数组慢，set把数值映射到key上要做hash计算，数据量大时差距很明显
- 解法：集合化后求交

# Solution

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        var nums = new HashSet<Integer>();
        var res = new HashSet<Integer>();

        for (int n : nums1){
            nums.add(n);
        }

        for (int n : nums2){
            if (nums.contains(n)){ res.add(n); }
        }

        return res.stream().mapToInt(Integer::intValue).toArray();

    }
}
```