---
type: template
tags: ["Graphs"]
---

# 0-1 BFS

**언제** 간선 비용이 정확히 0 또는 1일 때, Heap 없이 한 출발점의 최단거리를 구한다.

```text
0 ─1→ 1       Deque(정점:거리)
└─0→ 2 ─0→ 1  [2:0, 1:1] → [1:0, 1:1]
비용 0 → 앞(d), 비용 1 → 뒤(d+1)
1:0으로 개선 → 옛 1:1은 버림
```

**코드** `List<Node>[] graph`. Node.cost는 간선에서 **0/1 비용**, Deque에서는 **출발점부터 누적 거리**다. INF=`Long.MAX_VALUE/4`.

```java
static class Node {
    int vertex; long cost;
    Node(int v, long c) { vertex = v; cost = c; }
}
static long[] zeroOneBfs(int source) {
    long[] dist = new long[graph.length];
    Arrays.fill(dist, INF);
    Deque<Node> deque = new ArrayDeque<>();
    dist[source] = 0;
    deque.addFirst(new Node(source, 0));
    while (!deque.isEmpty()) {
        Node cur = deque.removeFirst();
        if (cur.cost != dist[cur.vertex]) continue;
        for (Node edge : graph[cur.vertex]) {
            long next = cur.cost + edge.cost;
            if (next >= dist[edge.vertex]) continue; // ①
            dist[edge.vertex] = next;
            Node candidate = new Node(edge.vertex, next);
            if (edge.cost == 0) deque.addFirst(candidate); // ②
            else deque.addLast(candidate);
        }
    }
    return dist;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 더 짧을 때만 삽입 | 같은 거리도 넣으면 비용 0 사이클에서 반복할 수 있다. |
| ② 0은 앞 · 1은 뒤 | 현재 거리 d의 후보를 d+1보다 먼저 처리한다. |
| 첫 삽입은 확정이 아님 | 나중에 발견한 비용 0 경로가 더 짧을 수 있다. |

**주의** 비용이 0/1 외의 값이면 Dijkstra 등을 사용한다. O(V+E) 시간·공간.

[[BFS]] · [[Dijkstra]]
