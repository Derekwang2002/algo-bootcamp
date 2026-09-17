# KamaCoder 104. 建造最大岛屿

Date: Mar 26
Level: Hard
Minutes: 120
Review Recommendation: 🚩 Recommend Redo (Hard >50m)
Status: Done
Tags: GraphTheory
Time Complexity: O(VE)
Space Complexity: O(VE)
URL: https://kamacoder.com/problempage.php?pid=1176
Carl's: https://programmercarl.com/kamacoder/0104.%E5%BB%BA%E9%80%A0%E6%9C%80%E5%A4%A7%E5%B2%9B%E5%B1%BF.html#%E6%80%9D%E8%B7%AF

<aside>
💡

给定一个由 1（陆地）和 0（水）组成的矩阵，你最多可以将矩阵中的一格水变为一块陆地，在执行了此操作之后，矩阵中最大的岛屿面积是多少。

岛屿面积的计算方式为组成岛屿的陆地的总数。岛屿是被水包围，并且通过水平方向或垂直方向上相邻的陆地连接而成的。你可以假设矩阵外均被水包围。

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

- mark each island a unique id(≥2), and store <id, area> into hash map
- Traverse 0(water), sum up its surrounding island area, get max

# Solution

```java
import java.util.*;

public class Main {
    static int[][] map;
    static boolean[][] visited;
    static Map<Integer, Integer> idMap = new HashMap<>();
    static Set<Integer> ids = new HashSet<>();
    static int[][] dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

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

        int id = 2;
        int area = 1;

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (map[i][j] == 1 && !visited[i][j]) {
                    area = bfs(i, j, id);
                    idMap.put(id, area);
                    id++;
                }
            }
        }
        
        int res = area;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (map[i][j] == 0) {
                    res = Math.max(res, maxConnect(i, j));
                    // System.out.println("-------" + i + ',' + j + "-----");
                }

            }
        }

        System.out.print(res);
    }

    static int bfs(int r, int c, int id) {
        Deque<int[]> queue = new ArrayDeque<>();
        queue.offer(new int[]{r, c});

        visited[r][c] = true;
        map[r][c] = id;

        int res = 1;
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            // System.out.println();

            for (int[] d : dirs) {
                int nr = cur[0] + d[0];
                int nc = cur[1] + d[1];

                if (nr < 0 || nr >= map.length || nc < 0 || nc >= map[0].length) continue;
                if (visited[nr][nc] || map[nr][nc] == 0) continue;

                queue.offer(new int[] {nr, nc});

                visited[nr][nc] = true;
                map[nr][nc] = id;
                res++;
            }
        }

        return res;
    }

    static int maxConnect(int r, int c) {
        int areas = 1;
        ids.clear();

        for (int k = 0; k < 4; k++) {
            int nr = r + dirs[k][0];
            int nc = c + dirs[k][1];
            if (nr < 0 || nr >= map.length || nc < 0 || nc >= map[0].length) continue;

            int id = map[nr][nc];
            if (id >= 2) ids.add(id);
        }

        for (int i : ids) areas += idMap.get(i);

        return areas;
    }
}

```

### Optimized

- eliminated `visited`
    - 0 → water
    - 1 → unvisited land
    - ≥2 → visited land
- Use `hasZero` to determine case that are all 1 in map
    - area would be `rows * cols`
- Create hashset for each 0

```java
import java.util.*;

public class Main {
    static int[][] grid;
    static int[][] dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    static Map<Integer, Integer> areaMap = new HashMap<>();

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);

        int rows = in.nextInt();
        int cols = in.nextInt();
        grid = new int[rows][cols];

        boolean hasZero = false;

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                grid[i][j] = in.nextInt();
                if (grid[i][j] == 0) hasZero = true;
            }
        }

        int id = 2;
        int res = 0;

        // 1. 给每个岛编号，并记录面积
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == 1) {
                    int area = bfs(i, j, id);
                    areaMap.put(id, area);
                    id++;
                }
            }
        }

        // 2. 如果全是1，直接输出整个岛面积
        if (!hasZero) {
            System.out.println(rows * cols);
            return;
        }

        // 3. 枚举每个0，尝试翻转
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == 0) {
                    res = Math.max(res, connectArea(i, j));
                }
            }
        }

        System.out.println(res);
    }

    static int bfs(int r, int c, int id) {
        Deque<int[]> queue = new ArrayDeque<>();
        queue.offer(new int[]{r, c});
        grid[r][c] = id;

        int area = 1;

        while (!queue.isEmpty()) {
            int[] cur = queue.poll();

            for (int[] d : dirs) {
                int nr = cur[0] + d[0];
                int nc = cur[1] + d[1];

                if (nr < 0 || nr >= grid.length || nc < 0 || nc >= grid[0].length) {
                    continue;
                }
                if (grid[nr][nc] != 1) {
                    continue;
                }

                grid[nr][nc] = id;
                queue.offer(new int[]{nr, nc});
                area++;
            }
        }

        return area;
    }

    static int connectArea(int r, int c) {
        int area = 1;
        Set<Integer> seen = new HashSet<>();

        for (int[] d : dirs) {
            int nr = r + d[0];
            int nc = c + d[1];

            if (nr < 0 || nr >= grid.length || nc < 0 || nc >= grid[0].length) {
                continue;
            }

            int id = grid[nr][nc];
            if (id > 1 && seen.add(id)) {
                area += areaMap.get(id);
            }
        }

        return area;
    }
}
```