---
type: template
tags: ["Search"]
aliases: ["Breadth-First Search"]
category: traversal-trees
categories: [traversal-trees, shortest-paths]
---

# BFS

```java
static int[][] bfs(int[][] map, Point start) {
    int H = map.length, W = map[0].length;
    int[] dx = {1, 0, -1, 0}, dy = {0, 1, 0, -1};
    int[][] dist = new int[H][W];
    for (int[] row : dist) Arrays.fill(row, -1);
    Queue<Point> queue = new ArrayDeque<>();
    dist[start.y][start.x] = 0;
    queue.offer(new Point(start));
    while (!queue.isEmpty()) {
        // pop: 가까운 칸부터 꺼내기
        Point cur = queue.poll();
        // expand: 상하좌우 후보 만들기
        for (int dir = 0; dir < 4; dir++) {
            int nx = cur.x + dx[dir], ny = cur.y + dy[dir];
            if (nx < 0 || nx >= W || ny < 0 || ny >= H) continue;
            if (map[ny][nx] == 0 || dist[ny][nx] != -1) continue;
            // visit: 길·미방문 칸의 거리 기록 후 등록
            dist[ny][nx] = dist[cur.y][cur.x] + 1;
            queue.offer(new Point(nx, ny));
        }
    }
    return dist;
}
```

비어 있지 않은 `map[y][x]`(1=길, 0=벽), 유효한 길 위의 `start`; `Point`는 `java.awt.Point`, x=열·y=행; 반환값은 이동 횟수, 도달 불가=-1. O(HW).

**분류** [[00 Maps/Traversal and Trees|Traversal and Trees]]

**비교** [[30 Search/DFS and Graphs|DFS and Graphs]]
