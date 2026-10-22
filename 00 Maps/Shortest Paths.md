---
type: map
level: family
category: shortest-paths
categories: [shortest-paths]
branch: graphs
---

# Shortest Paths

[[00 Maps/Graphs|Graphs]]

```text
출발점 ── 간선 비용을 더하며 이동 ──→ 목적지
          여러 경로 중 합이 가장 작은 경로
```

| 간선 비용 / 질문 | 고를 템플릿 | 다음에 처리할 대상 |
| --- | --- | --- |
| 모든 간선 비용 동일 | [[30 Search/BFS\|BFS]] | 먼저 도착한 순서 · Queue |
| 비용이 0 또는 1 | [[50 Graphs/0-1 BFS\|0-1 BFS]] | 0이면 앞, 1이면 뒤 · Deque |
| 비용이 모두 0 이상 | [[50 Graphs/Dijkstra\|Dijkstra]] | 현재 누적 거리가 최소 · Heap |
| 음수 간선 허용 / 음수 사이클 | [[50 Graphs/Bellman-Ford\|Bellman-Ford]] | 모든 간선을 여러 라운드 확인 |
| 모든 정점 쌍의 거리 | [[50 Graphs/Floyd-Warshall\|Floyd-Warshall]] | 경유할 정점을 하나씩 추가 |

Floyd-Warshall은 `O(V³)`이므로 정점 수를 먼저 본다. 음수 사이클이 끼는 경로에는 유한한 최단 거리가 없다.
