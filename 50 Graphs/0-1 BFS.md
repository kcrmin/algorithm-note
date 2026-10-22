---
type: template
tags: ["Graphs"]
category: shortest-paths
categories: [shortest-paths]
---

# 0-1 BFS

```java
static long[] zeroOneBfs(List<int[]>[] graph, int start) {
    long INF = Long.MAX_VALUE / 4;
    long[] dist = new long[graph.length];
    Arrays.fill(dist, INF);
    Deque<long[]> deque = new ArrayDeque<>();
    dist[start] = 0;
    deque.addFirst(new long[]{start, 0});
    while (!deque.isEmpty()) {
        long[] cur = deque.removeFirst();
        int v = (int) cur[0];
        if (cur[1] != dist[v]) continue; // 더 짧은 거리가 생긴 옛 후보는 버림
        for (int[] edge : graph[v]) {
            int to = edge[0];
            long next = cur[1] + edge[1];
            if (next >= dist[to]) continue; // 같은 거리 재삽입은 0비용 순환을 반복
            dist[to] = next;
            long[] state = {to, next};
            // 비용 0은 앞, 1은 뒤: 작은 거리부터 처리
            if (edge[1] == 0) deque.addFirst(state);
            else deque.addLast(state);
        }
    }
    return dist;
}
```

`graph[v]`의 간선 `{to, weight}`, weight=0 또는 1; 상태 `{vertex, distance}`, 도달 불가=`Long.MAX_VALUE/4`. O(V+E).

**분류** [[00 Maps/Shortest Paths|Shortest Paths]]

**비교** [[30 Search/BFS|BFS]]
