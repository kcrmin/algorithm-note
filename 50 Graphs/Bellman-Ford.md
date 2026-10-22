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
    // round: 모든 간선을 최대 V회 확인
    for (int round = 1; round <= n; round++) {
        boolean updated = false;
        for (long[] edge : edges) {
            int from = (int) edge[0], to = (int) edge[1];
            // skip: 출발점에서 못 닿는 간선 제외
            if (dist[from] == INF) continue;
            long next = dist[from] + edge[2];
            if (next >= dist[to]) continue;
            // relax: 더 짧은 거리로 갱신
            dist[to] = next;
            updated = true;
            // detect: V번째에도 줄어들면 음수 사이클
            if (round == n) return true;
        }
        // stop: 변화가 없으면 더 확인할 필요 없음
        if (!updated) return false;
    }
    return false;
}
```

`edges`: `List<long[]>`의 `{from,to,cost}`; `dist[source]=0`, 나머지 `INF=Long.MAX_VALUE/4`; true=닿는 음수 사이클, false면 dist 사용. 유효 거리·계산 합은 (-INF,INF). O(VE).

**분류** [[00 Maps/Shortest Paths|Shortest Paths]]
