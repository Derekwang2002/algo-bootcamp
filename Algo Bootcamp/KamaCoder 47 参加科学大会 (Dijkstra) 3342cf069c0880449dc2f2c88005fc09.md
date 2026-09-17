# KamaCoder 47. 参加科学大会 (Dijkstra)

Date: Mar 31
Level: Medium
Minutes: 40
Review Recommendation: ✅ Good job
Status: Done
Tags: GraphTheory, ShortestPath
Time Complexity: O(ElogE)
Space Complexity: O(V + E)
URL: https://kamacoder.com/problempage.php?pid=1047
Carl's: https://programmercarl.com/kamacoder/0047.%E5%8F%82%E4%BC%9Adijkstra%E6%9C%B4%E7%B4%A0.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

<aside>
💡

小明是一位科学家，他需要参加一场重要的国际科学大会，以展示自己的最新研究成果。

小明的起点是第一个车站，终点是最后一个车站。然而，途中的各个车站之间的道路状况、交通拥堵程度以及可能的自然因素（如天气变化）等不同，这些因素都会影响每条路径的通行时间。

小明希望能选择一条花费时间最少的路线，以确保他能够尽快到达目的地。

- input: 第一行包含两个正整数，第一个正整数 N 表示一共有 N 个公共汽车站，第二个正整数 M 表示有 M 条公路。接下来为 M 行，每行包括三个整数，S、E 和 V，代表了从 S 车站可以单向直达 E 车站，并且需要花费 V 单位的时间。
- output: 输出一个整数，代表小明从起点到终点所花费的最小时间。
</aside>

# Thought

- Dijkstra algo: [Dijkstra: ](https://app.notion.com/p/Dijkstra-3352cf069c088094a754c7810431280d?pvs=21)
- Detail:
    - Normal: when picking target node, it is possible that none of rest node are reachable, if so, break dijkstra loop.
    - Min-Heap optimization: possible that exist more than one record of a node in priority queue, do **lazy deletion** by:
        - `if (distOfNode > minDist[ndoe]) continue;`
- Complexity:
    - Normal:
        - time → $O(V^2)$
        - space → $O(V + E)$
    - Min-Heap opimize:
        - time → $O(ElogE)$
        - space → $O(V + E)$

# Solution

```java
import java.util.*;

public class Main {
    public static void main(String[] arga) {
        Scanner in = new Scanner(System.in);

        int n = in.nextInt();
        int m = in.nextInt();

        List<int[]>[] graph = new List[n + 1]; // {[to, weight]}
        int[] minDist = new int[n + 1]; // distance from start
        boolean[] visited = new boolean[n + 1];

        Arrays.fill(minDist, Integer.MAX_VALUE);
        minDist[1] = 0; 

        for (int i = 0; i <= n; i++) {
            graph[i] = new ArrayList<>();
        }

				// input
        while (m-- > 0) {
            int s = in.nextInt();
            int e = in.nextInt();
            int v = in.nextInt();

            graph[s].add(new int[]{e, v});
        }

				// dijkstra loop
        for (int i = 1; i <= n; i++) {
            int minVal = Integer.MAX_VALUE;
            int cur = -1; // possible that all rest are not reachable
            
            for (int v = 1; v <= n; v++) {
                if (!visited[v] && minDist[v] < minVal) {
                    minVal = minDist[v];
                    cur = v;
                }
            }

            if (cur == -1) break;
            visited[cur] = true;

            for (int[] e : graph[cur]) {
                int child = e[0];
                minDist[child] = Math.min(minDist[child], minDist[cur] + e[1]);
            }
        }

        if (minDist[n] == Integer.MAX_VALUE) {
            System.out.print(-1);
            return;
        }

        System.out.print(minDist[n]);
    }
}
```

### Min-Heap optimization

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
    public static void main(String[] arga) {
        Scanner in = new Scanner(System.in);

        int n = in.nextInt();
        int m = in.nextInt();

        List<Edge>[] graph = new List[n + 1];
        int[] minDist = new int[n + 1];

        Arrays.fill(minDist, Integer.MAX_VALUE);
        minDist[1] = 0; 

        for (int i = 0; i <= n; i++) {
            graph[i] = new ArrayList<>();
        }

        while (m-- > 0) {
            int s = in.nextInt();
            int e = in.nextInt();
            int v = in.nextInt();

            graph[s].add(new Edge(e, v)); // [to, weight]
        }

        PriorityQueue<Edge> pq = new PriorityQueue<>(
            Comparator.comparingInt(edge -> edge.val)
        );

        pq.offer(new Edge(1, 0));

        while (!pq.isEmpty()) {
            Edge cur = pq.poll();
            int idx = cur.to;
            int dist = cur.val;

            if (dist > minDist[idx]) continue; // lazy deletion

            for (Edge edge : graph[idx]) {
                if (minDist[idx] + edge.val < minDist[edge.to]) {
                    minDist[edge.to] = minDist[idx] + edge.val;
                    pq.offer(new Edge(edge.to, minDist[edge.to]));
                }
            }
        }

        System.out.print(minDist[n] == Integer.MAX_VALUE ? -1 : minDist[n]);
    }
}
```