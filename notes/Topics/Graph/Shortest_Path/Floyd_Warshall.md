---
type: algorithm
tags:
  - graph
  - shortest-path
  - floyd-warshall
language: java
status: in-review
source: []
---

# Floyd-Warshall (플로이드-워셜)

> **한 줄 요약:** 모든 정점 쌍 사이의 최단거리를 O(V³)에 구하는 알고리즘

## Core Concept

- **When to use:** 모든 쌍 최단경로, 간선 개수가 많아도 V가 작을 때 (보통 V 수백 이하)
- **Key Point:**
  - 중간 정점을 1..V까지 순서대로 허용하면서 갱신: `dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`
  - 음수 간선 가능 (음수 사이클이 있으면 i→i가 음수로 갱신됨)
  - 인접 행렬로 초기화: self 0, 간선 있으면 w, 없으면 INF

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Init | `dist[i][i]=0`, 간선 있으면 `dist[u][v]=w`, 나머지 INF | 인접 행렬 |
| Relax | `dist[i][j] = min(dist[i][j], dist[i][k]+dist[k][j])` | k를 거치는 경로 |
| Order | k를 1..V 순으로 돌림 | 중간 정점 순서 |

## Patterns

### Pattern A: 모든 쌍 최단거리

```java
long[][] dist = new long[n+1][n+1];
for (int i = 1; i <= n; i++) {
    Arrays.fill(dist[i], Long.MAX_VALUE);
    dist[i][i] = 0;
}
for (int u = 1; u <= n; u++) {
    for (Edge e : graph[u])
        dist[u][e.to] = Math.min(dist[u][e.to], e.w);
}

for (int k = 1; k <= n; k++) {
    for (int i = 1; i <= n; i++) {
        if (dist[i][k] == Long.MAX_VALUE) continue;
        for (int j = 1; j <= n; j++) {
            if (dist[k][j] == Long.MAX_VALUE) continue;
            if (dist[i][j] > dist[i][k] + dist[k][j])
                dist[i][j] = dist[i][k] + dist[k][j];
        }
    }
}
// dist[i][j] == Long.MAX_VALUE 이면 i->j 경로 없음
// 음수 사이클 있으면 dist[i][i] < 0 인 i 존재 가능
```

## Pitfalls & Tips

- INF와 덧셈 시 overflow 방지: 위처럼 INF인 경우 스킵
- 정점 번호가 0부터면 0..n-1로 인덱스 맞추기
- 경로 복원이 필요하면 parent[i][j]에 중간 정점 기록
- V가 크면 (수천 이상) O(V³)이 무거우니 다익스트라 N번 등 다른 방법 고려
