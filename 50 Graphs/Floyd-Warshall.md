---
type: template
tags: ["Graphs"]
---

# Floyd-Warshall

```java
// dist는 long[][]: 대각선=0, 없는 간선=INF, 중복 간선=최소 비용.
// INF=Long.MAX_VALUE/4. 유효 거리와 계산 중 합은 (-INF,INF) 범위.
// 계산 후 dist[v][v]<0이면 음수 사이클: 이를 거치는 쌍은 유한 최단거리가 없다.
static void floyd(int n) {
    // k가 바깥: 중간 정점을 0..k까지 허용한 거리표를 만듦
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            // 없는 경로에 음수 비용을 더해 가짜 경로가 되는 것 방지
            if (dist[i][k] == INF) continue;
            for (int j = 0; j < n; j++) {
                if (dist[k][j] == INF) continue;
                // 직행과 k 경유 중 더 싼 쪽
                dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
            }
        }
    }
}
```

**예시**

```text
직접: 0 ─10→ 2
      0 ─3→ 1 ─4→ 2
k=1을 경유점으로 허용
거리[0][2] = min(10, 3+4) = 7
```

**주의** 음수 사이클을 거칠 수 있는 쌍은 유한 최단거리가 없다. O(V³) 시간, O(V²) 공간.

[[Dijkstra]] · [[Bellman-Ford]]
