---
type: template
tags: ["Graphs"]
---

# Bellman-Ford

```java
// edges는 long[]{from,to,cost} 목록. dist[source]=0, 나머지는 INF로 시작.
// true는 출발점에서 닿는 음수 사이클. false일 때 dist가 최단거리.
static boolean hasNegativeCycle(int n) {
    // 단순 경로는 최대 V-1간선; V번째는 사이클 검사
    for (int round = 1; round <= n; round++) {
        boolean updated = false;
        for (long[] edge : edges) {
            int from = (int) edge[0], to = (int) edge[1];
            // 닿지 못한 곳의 INF+음수가 가짜 거리가 되지 않게 함
            if (dist[from] == INF) continue;
            long next = dist[from] + edge[2];
            if (next >= dist[to]) continue;
            dist[to] = next;
            updated = true;
            // V-1회 뒤에도 개선되려면 음수 사이클을 지나야 함
            if (round == n) return true;
        }
        // 모든 간선이 더는 개선 불가하면 이후 반복도 동일
        if (!updated) return false;
    }
    return false;
}
```

**예시**

```text
간선 입력 순서: 2→3(+2), 1→2(-3), 0→1(+4)
초기   [0, ∞, ∞, ∞]
1회    [0, 4, ∞, ∞] → 2회 [0,4,1,∞] → 3회 [0,4,1,3]
V-1회 뒤에도 거리 감소 → 닿을 수 있는 음수 사이클
```

**주의** true이면 일부 거리는 최단거리로 쓸 수 없다. 모든 합은 long 범위. O(VE) 시간, O(V) 거리 공간.

[[Dijkstra]] · [[Floyd-Warshall]]
