---
type: template
tags: ["Search"]
aliases: ["Breadth-First Search"]
---

# BFS

```java
// 비어 있지 않은 격자이며 시작 (0,0)과 도착 (W-1,H-1)은 길이어야 한다.
// 비용이 같은 격자: map[y][x]는 1=길, 0=벽. Point는 java.awt.Point.
// x=열, y=행. dist=0은 미방문이고, 답은 이동 횟수가 아닌 지나간 칸 수.
static int bfs(int[][] map) {
    int H = map.length, W = map[0].length;
    int[] dx = {1, 0, -1, 0}, dy = {0, 1, 0, -1};
    int[][] dist = new int[H][W];
    Queue<Point> queue = new ArrayDeque<>();
    dist[0][0] = 1; // 출발 칸도 1칸으로 세며 방문 표시
    queue.offer(new Point(0, 0));
    while (!queue.isEmpty()) { // 가까운 칸부터 꺼내므로 첫 발견 거리가 최단
        Point cur = queue.poll();
        if (cur.x == W - 1 && cur.y == H - 1) return dist[cur.y][cur.x];
        for (int dir = 0; dir < 4; dir++) {
            int nx = cur.x + dx[dir], ny = cur.y + dy[dir];
            // 배열을 읽기 전에 경계 검사
            if (nx < 0 || nx >= W || ny < 0 || ny >= H) continue;
            if (map[ny][nx] == 0 || dist[ny][nx] != 0) continue;
            // 넣기 전에 표시해야 같은 칸이 중복 삽입되지 않음
            dist[ny][nx] = dist[cur.y][cur.x] + 1;
            queue.offer(new Point(nx, ny));
        }
    }
    return -1;
}
```

**예시**

```text
모두 길인 2×2       발견 순서
거리: 1  2          (0,0):1 → 두 이웃:2 → (1,1):3
      2  3          Queue: 가까운 칸 → 먼 칸
답 3 = 지나간 칸 수 (이동 횟수는 2)
```

**주의** 비용이 다르면 이 방식으로 최단거리를 확정할 수 없다. O(HW) 시간·공간.

[[DFS and Graphs]] · [[0-1 BFS]] · [[Dijkstra]]
