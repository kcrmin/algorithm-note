---
type: template
tags: ["Graphs"]
---

# Dijkstra

```java
// 모든 간선 비용≥0. graph는 List<Node>[], INF=Long.MAX_VALUE/4.
// Node.cost는 graph에서 간선 비용, Heap에서 누적 거리. 도달 불가는 INF.
static class Node {
    int vertex; long cost;
    Node(int v, long c) { vertex = v; cost = c; }
}
static long[] dijkstra(int source) {
    long[] dist = new long[graph.length];
    Arrays.fill(dist, INF);
    // 누적 거리가 가장 작은 후보부터
    PriorityQueue<Node> pq = new PriorityQueue<>(Comparator.comparingLong(a -> a.cost));
    dist[source] = 0;
    pq.offer(new Node(source, 0));
    while (!pq.isEmpty()) {
        Node cur = pq.poll();
        // Heap에 남은 옛 후보 제거; 유효한 최소 후보의 거리는 확정
        if (cur.cost != dist[cur.vertex]) continue;
        for (Node edge : graph[cur.vertex]) {
            long next = cur.cost + edge.cost;
            // 더 짧은 경우에만 갱신해 같은 후보 반복 방지
            if (next >= dist[edge.vertex]) continue;
            dist[edge.vertex] = next;
            // Heap 안 옛 값을 고치지 않고 새 거리 후보를 추가
            pq.offer(new Node(edge.vertex, next));
        }
    }
    return dist;
}
```

**예시**

```text
0 ─8→ 1       Heap(정점:거리)
└─2→ 2 ─1→ 1  [2:2, 1:8] → [1:3, 1:8]
1:3을 먼저 꺼냄 → 거리 3 확정
1:8은 옛 후보라 버림
```

**주의** 음수 간선 제외. 도달 불가=INF, 거리 합은 INF 미만. O(V+E log(E+2)) 시간, O(V+E) 공간.

[[BFS]] · [[0-1 BFS]] · [[Bellman-Ford]] · [[Heap]]
