---
type: template
tags: ["Graphs"]
category: shortest-paths
categories: [shortest-paths]
---

# Floyd-Warshall

```java
static long[][] dist;
static final long INF = Long.MAX_VALUE / 4;

static void floyd(int n) {
    // allow: 사용할 수 있는 중간 정점 k를 하나씩 늘리기
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            // skip: 이어질 수 없는 경로 제외
            if (dist[i][k] == INF) continue;
            for (int j = 0; j < n; j++) {
                if (dist[k][j] == INF) continue;
                // relax: 직행과 k 경유 중 작은 값
                dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
            }
        }
    }
}
```

`long[][] dist`: 대각선 0·없는 간선 `INF=Long.MAX_VALUE/4`·중복 간선 최솟값; 합은 (-INF,INF); `dist[v][v]<0`이면 음수 사이클. O(V³).

**분류** [[00 Maps/Shortest Paths|Shortest Paths]]

**비교** [[50 Graphs/Dijkstra|Dijkstra]]
