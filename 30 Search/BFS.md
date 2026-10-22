---
type: template
tags: ["Search"]
aliases: ["Breadth-First Search"]
---

# BFS

**언제** 이동 비용이 모두 같을 때, 출발점에서 각 칸까지의 최단거리를 구한다.

```text
모두 길인 2×2       발견 순서
거리: 1  2          (0,0):1 → 두 이웃:2 → (1,1):3
      2  3          Queue: 가까운 칸 → 먼 칸
답 3 = 지나간 칸 수 (이동 횟수는 2)
```

**코드** `map[y][x]`: 1=길, 0=벽. `Point`는 `java.awt.Point`. 시작·도착은 길이다.

```java
static int bfs(int[][] map) {
    int H = map.length, W = map[0].length;
    int[] dx = {1, 0, -1, 0}, dy = {0, 1, 0, -1};
    int[][] dist = new int[H][W];
    Queue<Point> queue = new ArrayDeque<>();
    dist[0][0] = 1;
    queue.offer(new Point(0, 0));
    while (!queue.isEmpty()) {
        Point cur = queue.poll();
        if (cur.x == W - 1 && cur.y == H - 1) return dist[cur.y][cur.x];
        for (int dir = 0; dir < 4; dir++) {
            int nx = cur.x + dx[dir], ny = cur.y + dy[dir];
            if (nx < 0 || nx >= W || ny < 0 || ny >= H) continue;
            if (map[ny][nx] == 0 || dist[ny][nx] != 0) continue;
            dist[ny][nx] = dist[cur.y][cur.x] + 1; // ①
            queue.offer(new Point(nx, ny));
        }
    }
    return -1;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 넣기 전에 방문 표시 | 같은 칸을 여러 부모가 Queue에 넣는 것을 막는다. |
| 거리 = 부모 + 1 | Queue가 거리 순서를 지키므로 첫 발견이 최단이다. |

**주의** 비용이 다르면 이 방식으로 최단거리를 확정할 수 없다. O(HW) 시간·공간.

[[DFS and Graphs]] · [[0-1 BFS]] · [[Dijkstra]]
