---
type: template
tags: ["Graphs"]
category: shortest-paths
categories: [shortest-paths]
---

# Dijkstra

```java
static long[] dijkstra(List<int[]>[] graph, int start) {
    long INF = Long.MAX_VALUE / 4;
    long[] dist = new long[graph.length];
    Arrays.fill(dist, INF);
    // 상태는 {정점, 누적 거리}: 누적 거리가 작은 순서
    PriorityQueue<long[]> pq = new PriorityQueue<>(Comparator.comparingLong(a -> a[1]));
    dist[start] = 0;
    pq.offer(new long[]{start, 0});
    while (!pq.isEmpty()) {
        long[] cur = pq.poll();
        int v = (int) cur[0];
        if (cur[1] != dist[v]) continue; // 더 짧은 거리가 생긴 옛 후보는 버림
        for (int[] edge : graph[v]) {
            int to = edge[0];
            long next = cur[1] + edge[1];
            if (next >= dist[to]) continue; // 같을 때도 넣으면 0비용 순환을 반복
            dist[to] = next;
            pq.offer(new long[]{to, next});
        }
    }
    return dist;
}
```

`graph[v]`의 간선 `{to, weight}`, weight≥0인 int; 도달 불가=`Long.MAX_VALUE/4`, 거리 합은 그 미만. O(V+E log(E+2)).

**분류** [[00 Maps/Shortest Paths|Shortest Paths]]

**비교** [[50 Graphs/0-1 BFS|0-1 BFS]] · [[50 Graphs/Bellman-Ford|Bellman-Ford]]
