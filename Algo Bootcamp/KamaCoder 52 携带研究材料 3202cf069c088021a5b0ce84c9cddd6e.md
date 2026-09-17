# KamaCoder 52. 携带研究材料

Date: Mar 10
Level: Medium
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram, Knapsack
Time Complexity: O(mn)
Space Complexity: O(m)
URL: https://kamacoder.com/problempage.php?pid=1052
Carl's: https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html#%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85

<aside>
💡

小明是一位科学家，他需要参加一场重要的国际科学大会，以展示自己的最新研究成果。他需要带一些研究材料，但是他的行李箱空间有限。这些研究材料包括实验设备、文献资料和实验样本等等，它们各自占据不同的重量，并且具有不同的价值。

小明的行李箱所能承担的总重量是有限的，问小明应该如何抉择，才能携带最大价值的研究材料，每种研究材料可以选择无数次，并且可以重复选择。

- 输入：第一行包含两个整数，n，v，分别表示研究材料的种类和行李所能承担的总重量。接下来包含 n 行，每行两个整数 wi 和 vi，代表第 i 种研究材料的重量和价值
- 输出：输出一个整数，表示最大价值。
</aside>

# Thought

> FUll knapsack
> 
- Basically same as 0-1 knapsack
    - 0-1 knapsack could only use an item once
    - FUll knapsack could use an item infinitely times
- Difference: **DP equation(for case 2)**
    - $dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - weight[i]] + value[i])$
    - case 1: not use i → dp[i - 1][j]
    - **case 2**: use i → dp[i][j - weight[i]] + value[i]

# Solution

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        int v = in.nextInt();
        int res = 0;

        int[] weight = new int[n];
        int[] value = new int[n];
        int[] dp = new int[v + 1];

        for (int i = 0; i < n; i++) {
            weight[i] = in.nextInt();
            value[i] = in.nextInt();
        }

        in.close();

        for (int i = 0; i < n; i++) {
            for (int j = weight[i]; j <= v; j++) {
                dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
            }
        }

        System.out.print(dp[v]);
    }
}
```