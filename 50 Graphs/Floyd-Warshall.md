---
type: template
tags: ["Graphs"]
---

# Floyd-Warshall

```java
static void floyd(int n) {
    for (int k = 0; k < n; k++) { // ①
        for (int i = 0; i < n; i++) {
            if (dist[i][k] == INF) continue;
            for (int j = 0; j < n; j++) {
                if (dist[k][j] == INF) continue;
                dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
            }
        }
    }
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① k가 맨 바깥 | 각 회차는 중간 정점으로 0..k까지 허용한 거리표를 만든다. |
| 도착 가능한 두 경로만 합침 | INF에 음수를 더해 가짜 경로를 만드는 것을 막는다. |
| dist[i][i] < 0 | 계산 뒤 대각선이 음수이면 음수 사이클이 존재한다. |

**예시**

```text
직접: 0 ─10→ 2
      0 ─3→ 1 ─4→ 2
k=1을 경유점으로 허용
거리[0][2] = min(10, 3+4) = 7
```

**적용** 정점 수가 작고 모든 정점 쌍의 최단거리가 필요할 때 사용한다.

**준비** `long[][] dist`: 대각선=0, 없는 간선=INF, 중복 간선=최소 비용.

**주의** 음수 사이클을 거칠 수 있는 쌍은 유한 최단거리가 없다. O(V³) 시간, O(V²) 공간.

[[Dijkstra]] · [[Bellman-Ford]]
