# KamaCoder 94. 城市间货物运输 I (Bellman-Ford)

Date: Mar 31
Level: Medium
Minutes: 30
Review Recommendation: ✅ Good job
Status: Done
Tags: GraphTheory, ShortestPath
Time Complexity: O(VE)
Space Complexity: O(E)
URL: https://kamacoder.com/problempage.php?pid=1152
Carl's: https://programmercarl.com/kamacoder/0094.%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93I.html

<aside>
💡

某国为促进城市间经济交流，决定对货物运输提供补贴。共有 n 个编号为 1 到 n 的城市，通过道路网络连接，网络中的道路仅允许从某个城市单向通行到另一个城市，不能反向通行。

网络中的道路都有各自的运输成本和政府补贴，**道路的权值计算方式为：运输成本 - 政府补贴**。权值为正表示扣除了政府补贴后运输货物仍需支付的费用；权值为负则表示政府的补贴超过了支出的运输成本，实际表现为运输过程中还能赚取一定的收益。

请找出从城市 1 到城市 n 的所有可能路径中，综合政府补贴后的最低运输成本。如果最低运输成本是一个负数，它表示在遵循最优路径的情况下，运输过程中反而能够实现盈利。

**城市 1 到城市 n 之间可能会出现没有路径的情况，同时保证道路网络中不存在任何负权回路。**

- input: 第一行包含两个正整数，第一个正整数 n 表示该国一共有 n 个城市，第二个整数 m 表示这些城市中共有 m 条道路。接下来为 m 行，每行包括三个整数，s、t 和 v，表示 s 号城市运输货物到达 t 号城市，道路权值为 v （单向图）。
- output: 如果能够从城市 1 到连通到城市 n， 请输出一个整数，表示运输成本。如果该整数是负数，则表示实现了盈利。如果从城市 1 没有路径可达城市 n，请输出 "unconnected"。
</aside>

# Thought

- Bellman-ford(basic/Queue promoted) algo: [Bellman-Ford:](https://app.notion.com/p/Bellman-Ford-3352cf069c0880b68e2cd587ebcff9cb?pvs=21)

# Solution

```java
import java.util.*;

class Edge {
    int start;
    int to;
    int val;

    public Edge(int s, int t, int v) {
        start = s;
        to = t;
        val = v;
    }
}

public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);

        int n = in.nextInt();
        int m = in.nextInt();

        Edge[] edges = new Edge[m];
        int[] minDist = new int[n + 1];

        Arrays.fill(minDist, Integer.MAX_VALUE);
        minDist[1] = 0;

        for (int i = 0; i < m; i++) {
            int s = in.nextInt();
            int t = in.nextInt();
            int v = in.nextInt();

            edges[i] = new Edge(s, t, v);
        }

        for (int i = 0; i < n - 1; i++) {
            for (Edge e : edges) {
                int start = e.start;
                int to = e.to;
                int val = e.val;

                if (minDist[start] != Integer.MAX_VALUE && minDist[start] + val < minDist[to]) {
                    minDist[to] = minDist[start] + val;
                }
            }
        }

        System.out.print(minDist[n] == Integer.MAX_VALUE ? "unconnected" : minDist[n]);
    }
}
```

### Queue Promoted

```java
import java.util.*;

class Edge {
    int to;
    int val;

    public Edge(int t, int v) {
        to = t;
        val = v;
    }
}

public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);

        int n = in.nextInt();
        int m = in.nextInt();

        List<Edge>[] graph = new List[n + 1];
        int[] minDist = new int[n + 1];
        boolean[] visiting = new boolean[n + 1];

        Arrays.fill(minDist, Integer.MAX_VALUE);
        minDist[1] = 0;

        for (int i = 0; i <= n; i++) {
            graph[i] = new ArrayList<>();
        }

        for (int i = 0; i < m; i++) {
            int s = in.nextInt();
            int t = in.nextInt();
            int v = in.nextInt();

            graph[s].add(new Edge(t, v));
        }

        Deque<Integer> que = new ArrayDeque<>();
        que.offer(1);

        while (!que.isEmpty()) {
            int cur = que.poll();
            visiting[cur] = false;

            for (Edge e : graph[cur]) {
                if (minDist[cur] + e.val < minDist[e.to]) {
                    minDist[e.to] = minDist[cur] + e.val;

                    if (!visiting[e.to]) {
                        que.offer(e.to);
                        visiting[e.to] = true;
                    }
                }
            }
        }

        System.out.print(minDist[n] == Integer.MAX_VALUE ? "unconnected" : minDist[n]);
    }
}
```