---
type: template
tags: ["Graphs"]
---

# Bellman-Ford

```java
static boolean hasNegativeCycle(int n) {
    for (int round = 1; round <= n; round++) {
        boolean updated = false;
        for (long[] edge : edges) {
            int from = (int) edge[0], to = (int) edge[1];
            if (dist[from] == INF) continue; // ①
            long next = dist[from] + edge[2];
            if (next >= dist[to]) continue;
            dist[to] = next;
            updated = true;
            if (round == n) return true; // ②
        }
        if (!updated) return false;
    }
    return false;
}
```

| 암기 포인트 | 이유 |
|---|---|
| V-1번이면 충분 | 사이클 없는 경로는 정점을 반복하지 않아 간선이 최대 V-1개다. |
| ② V번째 개선 = 사이클 | 더 싼 경로에 정점 반복이 필요하므로 음수 사이클이다. |
| ① INF는 건너뜀 | 도달하지 않은 곳의 음수 간선이 가짜 거리를 만들지 못하게 한다. |

**예시**

```text
간선 입력 순서: 2→3(+2), 1→2(-3), 0→1(+4)
초기   [0, ∞, ∞, ∞]
1회    [0, 4, ∞, ∞] → 2회 [0,4,1,∞] → 3회 [0,4,1,3]
V-1회 뒤에도 거리 감소 → 닿을 수 있는 음수 사이클
```

**적용** 음수 간선이 있을 때 최단거리와 출발점에서 닿는 음수 사이클을 판정한다.

**준비** `edges`는 `long[]{from,to,cost}` 목록. `dist`는 INF로 채우고 `dist[source]=0`.

**주의** true이면 일부 거리는 최단거리로 쓸 수 없다. 모든 합은 long 범위. O(VE) 시간, O(V) 거리 공간.

[[Dijkstra]] · [[Floyd-Warshall]]
