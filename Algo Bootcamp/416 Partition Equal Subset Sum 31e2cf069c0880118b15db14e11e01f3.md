# 416. Partition Equal Subset Sum

Date: Mar 9
Level: Medium
Minutes: 60
Review Recommendation: 🚩 Recommend Redo (Medium >40m)
Status: Done
Tags: DynamicProgram, Knapsack
Time Complexity: O(mn)
Space Complexity: O(m)
URL: https://leetcode.com/problems/partition-equal-subset-sum/description/
Carl's: https://programmercarl.com/0416.%E5%88%86%E5%89%B2%E7%AD%89%E5%92%8C%E5%AD%90%E9%9B%86.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE

<aside>
💡

Given an integer array `nums`, return `true` *if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or* `false` *otherwise*.

</aside>

# Thought

- DP state: dp[i][j] = the maximum total value we can fill nums[0~i] into a knapsack with capacity j
- Base case: $dp[0][j] = nums[0] \quad \text{for all } j\ge nums[0]$
- **DP equation**:
    - Normal: $dp[i][j] = max\{dp[i - 1][j],  dp[i - 1][j - nums[i]] + nums[i]\}$
        - weight/value are both nums[i]
    - Compressed: $row[i] = \max(dp[i], dp[i - n] + n)$
        - `row` is next row, `dp` is previous row.
        - backward filling
- DP table: (for `{1, 5, 11, 5}`)

```
capacity →    0 1 2 3 4 5 6 7 8 9 10 11
items used ↓ --------------------------------
{}            0 0 0 0 0 0 0 0 0 0  0  0
{1}           0 1 1 1 1 1 1 1 1 1  1  1
{1,5}         0 1 1 1 1 5 6 6 6 6  6  6
{1,5,11}      0 1 1 1 1 5 6 6 6 6  6 11
{1,5,11,5}    0 1 1 1 1 5 6 6 6 6 10 11
```

- **Optimal(Rolling arrays)**:
    - **No Explicit Base Case Loop:** We no longer need to initialize the first row separately. The outer loop naturally handles `nums[0]` on its first pass.
    - **No Cloning:** `dp.clone()` is an $O(S)$ operation. Removing it drastically reduces the constant overhead and garbage collection in Java.
    - **Space Complexity is strictly $O(S)$:** We only maintain a single 1D array of size `sum / 2 + 1` in memory.
    - **Loop Condition:** The inner loop stops at `i >= n`. We don't need an `if (n > i) continue;` check because if the capacity $i$ is smaller than the current number $n$, we can't fit it in the knapsack anyway, so the value would just remain $dp[i]$.
- Boolean DP array:
    - dp[i][j] means: **if** the knapsack could be **fully filled** by nums from *0_th num to i_th num*.

# Solution

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int i : nums) sum += i;
        if (sum % 2 != 0) return false;

        int size = sum / 2; // knapsack size
        int[] dp = new int[size + 1];

        for (int i = 1; i <= size; i++) { // knapsack size
            if (nums[0] > i) continue;
            dp[i] = nums[0];
        }

        for (int j = 1; j < nums.length; j++) {
            int n = nums[j];
            if (n == size) return true;
            int[] row = dp.clone(); // hard copy of dp

            for (int i = 1; i <= size; i++) { // knapsack size
                if (n > i) continue; // overweight
                row[i] = Math.max(dp[i], dp[i - n] + n);
            }

            dp = row;
        }

        return dp[size] == size;
    }
}
```

### Optimization:

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int i : nums) sum += i;
        
        // If the total sum is odd, it cannot be partitioned into two equal integers
        if (sum % 2 != 0) return false;

        int size = sum / 2; // target knapsack capacity
        int[] dp = new int[size + 1];

        // Process each number in the array
        for (int n : nums) {
            // Iterate BACKWARDS from the max capacity down to 'n'
            for (int i = size; i >= n; i--) {
                dp[i] = Math.max(dp[i], dp[i - n] + n);
            }
            
            // Optional early exit: if we've already reached the target size, stop
            if (dp[size] == size) return true;
        }

        return dp[size] == size;
    }
}
```

### Original (boolean array)

```java
public class Solution {
    public static void main(String[] args) {
        int num[] = {1,5,11,5};
        canPartition(num);

    }
    public static boolean canPartition(int[] nums) {
        int len = nums.length;
        // 题目已经说非空数组，可以不做非空判断
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        // 特判：如果是奇数，就不符合要求
        if ((sum %2 ) != 0) {
            return false;
        }

        int target = sum / 2; //目标背包容量
        // 创建二维状态数组，行：物品索引，列：容量（包括 0）
        /*
        dp[i][j]表示从数组的 [0, i] 这个子区间内挑选一些正整数
          每个数只能用一次，使得这些数的和恰好等于 j。
        */
        boolean[][] dp = new boolean[len][target + 1];

        // 先填表格第 0 行，第 1 个数只能让容积为它自己的背包恰好装满  （这里的dp[][]数组的含义就是“恰好”，所以就算容积比它大的也不要）
        if (nums[0] <= target) {
            dp[0][nums[0]] = true;
        }
        // 再填表格后面几行
        //外层遍历物品
        for (int i = 1; i < len; i++) {
            //内层遍历背包
            for (int j = 0; j <= target; j++) {
                // 直接从上一行先把结果抄下来，然后再修正
                dp[i][j] = dp[i - 1][j];

                //如果某个物品单独的重量恰好就等于背包的重量，那么也是满足dp数组的定义的
                if (nums[i] == j) {
                    dp[i][j] = true;
                    continue;
                }
                //如果某个物品的重量小于j，那就可以看该物品是否放入背包
                //dp[i - 1][j]表示该物品不放入背包，如果在 [0, i - 1] 这个子区间内已经有一部分元素，使得它们的和为 j ，那么 dp[i][j] = true；
                //dp[i - 1][j - nums[i]]表示该物品放入背包。如果在 [0, i - 1] 这个子区间内就得找到一部分元素，使得它们的和为 j - nums[i]。
                if (nums[i] < j) {
                    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i]];
                }
            }
        }
        for (int i = 0; i < len; i++) {
            for (int j = 0; j <= target; j++) {
                System.out.print(dp[i][j]+" ");
            }
            System.out.println();
        }
        return dp[len - 1][target];
    }
}
//dp数组的打印结果
false true false false false false false false false false false false
false true false false false true true false false false false false
false true false false false true true false false false false true
false true false false false true true false false false true true
```