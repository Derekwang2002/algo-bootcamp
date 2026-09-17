# KamaCoder 95. 城市间货物运输 II (BF neg circle)

Date: Apr 2
Level: Medium
Minutes: 5
Review Recommendation: ✅ Good job
Status: Done
Tags: GraphTheory, ShortestPath
Time Complexity: O(VE)
Space Complexity: O(V + E)
URL: https://kamacoder.com/problempage.php?pid=1153

<aside>
💡

某国为促进城市间经济交流，决定对货物运输提供补贴。共有 n 个编号为 1 到 n 的城市，通过道路网络连接，网络中的道路仅允许从某个城市单向通行到另一个城市，不能反向通行。

网络中的道路都有各自的运输成本和政府补贴，**道路的权值计算方式为：运输成本 - 政府补贴**。权值为正表示扣除了政府补贴后运输货物仍需支付的费用；权值为负则表示政府的补贴超过了支出的运输成本，实际表现为运输过程中还能赚取一定的收益。

然而，在评估从城市 1 到城市 n 的所有可能路径中综合政府补贴后的最低运输成本时，存在一种情况：**图中可能出现负权回路。**负权回路是指一系列道路的总权值为负，这样的回路使得通过反复经过回路中的道路，理论上可以无限地减少总成本或无限地增加总收益。为了避免货物运输商采用负权回路这种情况无限的赚取政府补贴，算法还需检测这种特殊情况。

请找出从城市 1 到城市 n 的所有可能路径中，综合政府补贴后的最低运输成本。同时能够检测并适当处理负权回路的存在。

**城市 1 到城市 n 之间可能会出现没有路径的情况**

- input: 第一行包含两个正整数，第一个正整数 n 表示该国一共有 n 个城市，第二个整数 m 表示这些城市中共有 m 条道路。接下来为 m 行，每行包括三个整数，s、t 和 v，表示 s 号城市运输货物到达 t 号城市，道路权值为 v。
- output: 如果没有发现负权回路，则输出一个整数，表示从城市 `1` 到城市 `n` 的最低运输成本（包括政府补贴）。如果该整数是负数，则表示实现了盈利。如果发现了负权回路的存在，则输出 "circle"。如果从城市 1 无法到达城市 n，则输出 "unconnected"。
</aside>

# Thought

- Based on: [KamaCoder **94. 城市间货物运输 I (Bellman-Ford)**](KamaCoder%2094%20%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93%20I%20(Bellman-Ford)%203352cf069c0880a28448c83f6097bb6c.md)
- At n time, chekc if any of minDist decreased, if so, there is a negetive weight circle

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

        boolean flag = false;

        for (int i = 0; i < n; i++) { // traverse one more time
            for (Edge e : edges) {
                int start = e.start;
                int to = e.to;
                int val = e.val;

                if (minDist[start] != Integer.MAX_VALUE && minDist[start] + val < minDist[to]) {
                    minDist[to] = minDist[start] + val;

                    if (i == n - 1) {
                        flag = true;
                        break;
                    }
                }
            }
        }

        if (flag) {
            System.out.print("circle");
            return;
        }

        System.out.print(minDist[n] == Integer.MAX_VALUE ? "unconnected" : minDist[n]);
    }
}
```

### Queue Promoted

```java
import java.util.*;

public class Main {
    // 基于Bellman_ford-SPFA方法
    // Define an inner class Edge
    static class Edge {
        int from;
        int to;
        int val;
        public Edge(int from, int to, int val) {
            this.from = from;
            this.to = to;
            this.val = val;
        }
    }

    public static void main(String[] args) {
        // Input processing
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        List<List<Edge>> graph = new ArrayList<>();

        for (int i = 0; i <= n; i++) {
            graph.add(new ArrayList<>());
        }

        for (int i = 0; i < m; i++) {
            int from = sc.nextInt();
            int to = sc.nextInt();
            int val = sc.nextInt();
            graph.get(from).add(new Edge(from, to, val));
        }

        // Declare the minDist array to record the minimum distance form current node to the original node
        int[] minDist = new int[n + 1];
        Arrays.fill(minDist, Integer.MAX_VALUE);
        minDist[1] = 0;

        // Declare a queue to store the updated nodes instead of traversing all nodes each loop for more efficiency
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(1);

        // Declare an array to record the times each node has been offered in the queue
        int[] count = new int[n + 1];
        count[1]++;

        // Declare a boolean array to record if the current node is in the queue to optimise the processing
        boolean[] isInQueue = new boolean[n + 1];

        // Declare a boolean value to check if there is a negative weight loop inside the graph
        boolean flag = false;

        while (!queue.isEmpty()) {
            int curNode = queue.poll();
            isInQueue[curNode] = false; // Represents the current node is not in the queue after being polled
            for (Edge edge : graph.get(curNode)) {
                if (minDist[edge.to] > minDist[edge.from] + edge.val) { // Start relaxing the edge
                    minDist[edge.to] = minDist[edge.from] + edge.val;
                    if (!isInQueue[edge.to]) { // Don't add the node if it's already in the queue
                        queue.offer(edge.to);
                        count[edge.to]++;
                        isInQueue[edge.to] = true;
                    }

                    if (count[edge.to] == n) { // If some node has been offered in the queue more than n-1 times
                        flag = true;
                        while (!queue.isEmpty()) queue.poll();
                        break;
                    }
                }
            }
        }

        if (flag) {
            System.out.println("circle");
        } else if (minDist[n] == Integer.MAX_VALUE) {
            System.out.println("unconnected");
        } else {
            System.out.println(minDist[n]);
        }
    }
}
```