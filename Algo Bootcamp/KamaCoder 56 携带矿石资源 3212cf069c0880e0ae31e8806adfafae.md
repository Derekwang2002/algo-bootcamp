# KamaCoder 56. 携带矿石资源

Date: Mar 11
Level: Medium
Minutes: 10
Review Recommendation: ✅ Good job
Status: Done
Tags: DynamicProgram, Knapsack
Time Complexity: O(kmn )
Space Complexity: O(m)
URL: https://kamacoder.com/problempage.php?pid=1066
Carl's: https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%A4%9A%E9%87%8D%E8%83%8C%E5%8C%85.html#%E5%A4%9A%E9%87%8D%E8%83%8C%E5%8C%85

<aside>
💡

你是一名宇航员，即将前往一个遥远的行星。在这个行星上，有许多不同类型的矿石资源，每种矿石都有不同的重要性和价值。你需要选择哪些矿石带回地球，但你的宇航舱有一定的容量限制。

给定一个宇航舱，最大容量为 C。现在有 N 种不同类型的矿石，每种矿石有一个重量 w[i]，一个价值 v[i]，以及最多 k[i] 个可用。不同类型的矿石在地球上的市场价值不同。你需要计算如何在不超过宇航舱容量的情况下，最大化你所能获取的总价值。

- input:
    
    输入共包括四行，第一行包含两个整数 C 和 N，分别表示宇航舱的容量和矿石的种类数量。
    
    接下来的三行，每行包含 N 个正整数。具体如下：
    
    第二行包含 N 个整数，表示 N 种矿石的重量。
    
    第三行包含 N 个整数，表示 N 种矿石的价格。
    
    第四行包含 N 个整数，表示 N 种矿石的可用数量上限。
    
- output: 输出一个整数，代表获取的最大价值。
</aside>

# Thought

> Multiple knapsack
> 
- Based on 0-1 knapsack, add inner layer iteration to traverse possible numbers of item.

# Solution

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int c = in.nextInt(); // capacity
        int n = in.nextInt(); // #items

        int[] w = new int[n]; // weight
        int[] v = new int[n]; // value
        int[] k = new int[n]; // number of items

        for (int i = 0; i < n; i++) w[i] = in.nextInt();
        for (int i = 0; i < n; i++) v[i] = in.nextInt();
        for (int i = 0; i < n; i++) k[i] = in.nextInt();

        int[] dp = new int[c + 1];

        for (int i = 0; i < n; i++) {
            for (int j = c; j >= w[i]; j--) {
                for (int x = 1; x <= k[i] && (j - x * w[i] >= 0); x++) {
                    dp[j] = Math.max(dp[j], dp[j - x * w[i]] + x * v[i]);
                }
            }
        }

        System.out.println(dp[c]);
    }
}
```