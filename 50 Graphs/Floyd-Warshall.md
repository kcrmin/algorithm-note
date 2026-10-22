---
type: template
tags: ["Graphs"]
category: shortest-paths
categories: [shortest-paths]
---

# Floyd-Warshall

```text
    8
A ─────→ C
│        ↑
3        2
↓        │
B ───────┘

중간 정점 B를 허용:
거리[A][C] = min(8, 3+2) = 5

모든 출발점 × 모든 도착점에 반복
```

| Step | Pseudocode |
|---|---|
| Allow | // 중간에 거칠 수 있는 정점을 하나 더 허용한다. |
| Connect | // 출발→중간, 중간→도착 경로가 모두 있는지 본다. |
| Relax | // 기존 거리와 중간 정점을 거친 거리 중 작은 값을 저장한다. |

## Java

```java
static long[][] dist;
static final long INF = Long.MAX_VALUE / 4;

static void floyd(int n) {
    // 중간 정점의 허용 범위를 바깥 루프에서 늘린다.
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            if (dist[i][k] == INF) {
                continue;
            }

            for (int j = 0; j < n; j++) {
                if (dist[k][j] == INF) {
                    continue;
                }

                dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
            }
        }
    }
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| `k → i → j` 순서 | k까지의 중간 정점을 허용한 답을 만든다. 중간 정점 k를 가장 바깥에 둔다. |
| 초기 거리 | 대각선은 0, 없는 간선은 INF. 중복 간선은 최솟값, 음수 자기 루프가 있으면 그 값도 반영한다. |
| 두 경로의 INF 검사 | 연결되지 않은 두 구간을 더해서 가짜 경로를 만들지 않는다. |
| 음수 사이클 | 계산 후 dist[v][v]<0이면 존재한다. 그 사이클을 경유할 수 있는 쌍에는 유한 최단 거리가 없다. |

`long[][] dist`를 직접 변경. INF=`Long.MAX_VALUE/4`, 합은 (-INF,INF). 음수 간선 허용. **O(V³)** 시간·**O(V²)** 공간.

**분류** [[00 Maps/Shortest Paths|Shortest Paths]]

**비교** [[50 Graphs/Dijkstra|Dijkstra]]
