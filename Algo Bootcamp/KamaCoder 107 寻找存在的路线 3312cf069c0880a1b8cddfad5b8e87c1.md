# KamaCoder 107. 寻找存在的路线

Date: Mar 28
Level: Easy
Minutes: 10
Review Recommendation: ✅ Good job
Status: Done
Tags: GraphTheory, Union-Find Set
Time Complexity: O(n)
Space Complexity: O(n)
URL: https://kamacoder.com/problempage.php?pid=1179
Carl's: https://programmercarl.com/kamacoder/0107.%E5%AF%BB%E6%89%BE%E5%AD%98%E5%9C%A8%E7%9A%84%E8%B7%AF%E5%BE%84.html#%E6%80%9D%E8%B7%AF

<aside>
💡

给定一个包含 n 个节点的无向图中，节点编号从 1 到 n （含 1 和 n ）。

你的任务是判断是否有一条从节点 source 出发到节点 destination 的路径存在。

- input: 第一行包含两个正整数 N 和 M，N 代表节点的个数，M 代表边的个数。后续 M 行，每行两个正整数 s 和 t，代表从节点 s 与节点 t 之间有一条边。最后一行包含两个正整数，代表起始节点 source 和目标节点 destination。
- output: 输出一个整数，代表是否存在从节点 source 到节点 destination 的路径。如果存在，输出 1；否则，输出 0。
</aside>

# Thought

- Basic union-find problem
    - Code template: [Union Find Code](https://app.notion.com/p/Union-Find-Code-3322cf069c08808c84a3d03441cf6182?pvs=21)
    - Theory: https://programmercarl.com/kamacoder/%E5%9B%BE%E8%AE%BA%E5%B9%B6%E6%9F%A5%E9%9B%86%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html

# Solution

```java
import java.util.*;

class UnionFind {
    int[] parent;

    public UnionFind(int n) {
        parent = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            parent[i] = i;
        }
    }

    public int find(int x) {
        if (parent[x] == x) return x;
        return parent[x] = find(parent[x]);
    }

    public boolean isConnected(int u, int v) {
        return find(u) == find(v);
    }

    public void union(int u, int v) {
        u = find(u);
        v = find(v);

        if (u == v) return;
        parent[v] = u;
    }
}

public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);

        int n = in.nextInt();
        int m = in.nextInt();
        UnionFind uf = new UnionFind(n);

        for (int i = 0; i < m; i++) {
            uf.union(in.nextInt(), in.nextInt());
        }

        int source = in.nextInt();
        int dest = in.nextInt();

        if (uf.isConnected(dest, source)) {
            System.out.print(1);
            return;
        }

        System.out.print(0);
    }
}
```