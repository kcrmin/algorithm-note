---
type: template
tags: ["Graphs"]
---

# Dijkstra

**언제** 간선 비용이 모두 0 이상일 때, 한 출발점에서의 최단거리를 구한다.

```text
0 ─8→ 1       Heap(정점:거리)
└─2→ 2 ─1→ 1  [2:2, 1:8] → [1:3, 1:8]
1:3을 먼저 꺼냄 → 거리 3 확정
1:8은 옛 후보라 버림
```

**코드** `List<Node>[] graph`; 간선 Node의 cost는 비용, Heap Node의 cost는 누적 거리. INF는 `Long.MAX_VALUE/4`.

```java
static class Node {
    int vertex; long cost;
    Node(int v, long c) { vertex = v; cost = c; }
}
static long[] dijkstra(int source) {
    long[] dist = new long[graph.length];
    Arrays.fill(dist, INF);
    PriorityQueue<Node> pq = new PriorityQueue<>(Comparator.comparingLong(a -> a.cost));
    dist[source] = 0;
    pq.offer(new Node(source, 0));
    while (!pq.isEmpty()) {
        Node cur = pq.poll();
        if (cur.cost != dist[cur.vertex]) continue; // ①
        for (Node edge : graph[cur.vertex]) {
            long next = cur.cost + edge.cost;
            if (next >= dist[edge.vertex]) continue;
            dist[edge.vertex] = next;
            pq.offer(new Node(edge.vertex, next)); // ②
        }
    }
    return dist;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 옛 후보 제거 | Heap에는 넣을 당시의 거리가 남아 있으므로 현재 dist와 비교한다. |
| ② 개선할 때 다시 넣기 | 음수가 없어 최소 유효 후보를 꺼낼 때 그 거리가 확정된다. |
| 목적지에서 조기 종료 | Heap에 넣을 때가 아니라 유효 후보로 꺼낸 뒤 종료한다. |

**주의** 음수 간선 제외. 도달 불가=INF, 거리 합은 INF 미만. O(V+E log(E+2)) 시간, O(V+E) 공간.

[[BFS]] · [[0-1 BFS]] · [[Bellman-Ford]] · [[Heap]]
