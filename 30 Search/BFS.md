---
type: template
tags: ["Search"]
aliases: ["Breadth-First Search"]
category: traversal-trees
categories: [traversal-trees, shortest-paths]
---

# BFS

```text
한 칸 이동 = 비용 1
S  ·  ·       0  1  2
·  #  ·   →   1  #  3
·  ·  ·       2  3  4

Queue: 거리 0 → 거리 1 → 거리 2 → …
```

| Step | Pseudocode |
|---|---|
| Seed | // 시작 칸의 거리를 0으로 기록하고 큐에 넣는다. |
| Pop | // 먼저 들어온 칸부터 꺼낸다. 가까운 거리부터 처리된다. |
| Expand | // 상하좌우 중 범위 안·길·미방문인 칸만 고른다. |
| Visit | // 현재 거리보다 한 칸 멀다고 기록한 뒤 큐에 넣는다. |
| Answer | // 각 칸의 최단 이동 횟수를 반환한다. |

## Java

```java
static int[][] bfs(int[][] map, Point start) {
    int H = map.length, W = map[0].length;
    int[] dx = {1, 0, -1, 0}, dy = {0, 1, 0, -1};
    int[][] dist = new int[H][W];

    for (int[] row : dist) {
        Arrays.fill(row, -1);
    }

    Queue<Point> queue = new ArrayDeque<>();
    dist[start.y][start.x] = 0;
    queue.offer(new Point(start));

    while (!queue.isEmpty()) {
        Point cur = queue.poll();

        for (int dir = 0; dir < 4; dir++) {
            int nx = cur.x + dx[dir], ny = cur.y + dy[dir];
            if (nx < 0 || nx >= W || ny < 0 || ny >= H) {
                continue;
            }

            if (map[ny][nx] == 0 || dist[ny][nx] != -1) {
                continue;
            }

            // 큐에 넣기 전에 방문을 확정한다.
            dist[ny][nx] = dist[cur.y][cur.x] + 1;
            queue.offer(new Point(nx, ny));
        }
    }

    return dist;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| 방문 표시 → 큐 삽입 | 삽입할 때 표시해야 같은 칸이 여러 번 들어가지 않는다. |
| FIFO와 동일한 비용 | 먼저 발견한 경로가 최단인 이유. 가중치가 다르면 별도 알고리즘이 필요하다. |
| `map[y][x]`, `dist=-1` | x는 열, y는 행. 0은 시작 거리이므로 미방문 표시는 -1로 구분한다. |

`map`은 비어 있지 않은 격자(1=길, 0=벽), `start`는 길 위의 유효 좌표. `Point`는 `java.awt.Point`. 반환: 거리 배열, 도달 불가=-1. **O(HW)**.

**분류** [[00 Maps/Traversal and Trees|Traversal and Trees]]

**비교** [[30 Search/DFS and Graphs|DFS and Graphs]]
