# KamaCoder 99. 计数孤岛

Date: Mar 24
Level: Medium
Minutes: 40
Review Recommendation: ✅ Good job
Status: Done
Tags: GraphTheory
Time Complexity: O(VE)
Space Complexity: O(VE)
URL: https://kamacoder.com/problempage.php?pid=1171
Carl's: https://programmercarl.com/kamacoder/0099.%E5%B2%9B%E5%B1%BF%E7%9A%84%E6%95%B0%E9%87%8F%E6%B7%B1%E6%90%9C.html#%E6%80%9D%E8%B7%AF

<aside>
💡

给定一个由 1（陆地）和 0（水）组成的矩阵，你需要计算岛屿的数量。岛屿由水平方向或垂直方向上相邻的陆地连接而成，并且四周都是水域。你可以假设矩阵外均被水包围。

- input: 第一行包含两个整数 N, M，表示矩阵的行数和列数。后续 N 行，每行包含 M 个数字，数字为 1 或者 0。
- output: 输出一个整数，表示岛屿的数量。如果不存在岛屿，则输出 0。
- example：

```
4 5
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
```

</aside>

# Thought

- Store input → int[][] map
- Search design: start at ‘1’ place and stop when all surrouded by ‘0’
    - after visiting a place, record in a boolean[][] matrix.
- Traverse input and do BFS/DFS on not-visited ‘1’ place
    - after a search finish(returned), counter + 1.

# Solution

### BFS

```java
import java.util.*;

public class Main {
    static int[][] map;
    static boolean[][] visited;
    static int[][] directions = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}};

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);

        int rows = in.nextInt();
        int cols = in.nextInt();
        map = new int[rows][cols];
        visited = new boolean[rows][cols];

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                map[i][j] = in.nextInt();
            }
        }

        int count = 0;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (!visited[i][j] && map[i][j] == 1) count += bfs(i, j);
            }
        }

        System.out.print(count);
    }

    private static int bfs(int r, int c) {
        Deque<int[]> queue = new ArrayDeque<>();
        queue.offer(new int[]{r, c});
        visited[r][c] = true;

        while (!queue.isEmpty()) {
            int[] cur = queue.poll();

            for (int i = 0; i < 4; i++) {
                int newR = cur[0] + directions[i][0];
                int newC = cur[1] + directions[i][1];
                
                if (newR < 0 || newR > map.length - 1) continue;
                if (newC < 0 || newC > map[0].length - 1) continue;
                if (visited[newR][newC] || map[newR][newC] == 0) continue;

                queue.offer(new int[]{newR, newC});
                visited[newR][newC] = true;

            }
        }
        
        return 1;
    }
}
```

### DFS

```java
import java.util.*;

public class Main {
    static int[][] map;
    static boolean[][] visited;
    static int[][] directions = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}};

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);

        int rows = in.nextInt();
        int cols = in.nextInt();
        map = new int[rows][cols];
        visited = new boolean[rows][cols];

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                map[i][j] = in.nextInt();
            }
        }

        int count = 0;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (!visited[i][j] && map[i][j] == 1) count += dfs(i, j);
            }
        }

        System.out.print(count);
    }

    private static int dfs(int r, int c) {
        for (int i = 0; i < 4; i++) {
            int newR = r + directions[i][0];
            int newC = c + directions[i][1];

            if (newR < 0 || newR > map.length - 1) continue;
            if (newC < 0 || newC > map[0].length - 1) continue;
            if (visited[newR][newC] || map[newR][newC] == 0) continue;

            visited[newR][newC] = true;
            dfs(newR, newC);
        }

        return 1;
    }
```