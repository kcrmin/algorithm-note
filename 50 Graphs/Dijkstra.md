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
    PriorityQueue<long[]> pq = new PriorityQueue<>(Comparator.comparingLong(a -> a[1]));
    dist[start] = 0;
    pq.offer(new long[]{start, 0});
    while (!pq.isEmpty()) {
        // pop: {정점, 거리} 후보 중 거리가 가장 작은 것
        long[] cur = pq.poll();
        int v = (int) cur[0];
        // skip: 현재 거리와 다른 옛 후보 버리기
        if (cur[1] != dist[v]) continue;
        // relax: 더 짧을 때만 갱신 후 새 후보 등록
        for (int[] edge : graph[v]) {
            int to = edge[0];
            long next = cur[1] + edge[1];
            if (next >= dist[to]) continue;
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
