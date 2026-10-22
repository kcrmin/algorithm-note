---
type: template
tags: ["Graphs"]
category: shortest-paths
categories: [shortest-paths]
---

# Bellman-Ford

```java
static List<long[]> edges;
static long[] dist;
static final long INF = Long.MAX_VALUE / 4;

static boolean hasNegativeCycle(int n) {
    for (int round = 1; round <= n; round++) {
        boolean updated = false;
        for (long[] edge : edges) {
            int from = (int) edge[0], to = (int) edge[1];
            if (dist[from] == INF) continue; // INF+음수가 가짜 경로가 되지 않게 함
            long next = dist[from] + edge[2];
            if (next >= dist[to]) continue;
            dist[to] = next;
            updated = true;
            if (round == n) return true; // 단순 경로는 V-1간선: V번째 개선은 음수 사이클
        }
        if (!updated) return false;
    }
    return false;
}
```

`edges`: `List<long[]>`의 `{from,to,cost}`; `dist[source]=0`, 나머지 `INF=Long.MAX_VALUE/4`; true=닿는 음수 사이클, false면 dist 사용. 유효 거리·계산 합은 (-INF,INF). O(VE).

**분류** [[00 Maps/Shortest Paths|Shortest Paths]]
