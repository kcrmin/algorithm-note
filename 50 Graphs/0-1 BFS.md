---
type: template
tags: ["Graphs"]
---

# 0-1 BFS

```java
// 간선 비용은 0/1만 허용. graph는 List<Node>[], INF=Long.MAX_VALUE/4.
// Node.cost는 graph에서 간선 비용, deque에서 출발점부터의 누적 거리.
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
        // 더 짧은 후보가 생긴 뒤 남은 옛 거리 제거
        if (cur.cost != dist[cur.vertex]) continue;
        for (Node edge : graph[cur.vertex]) {
            long next = cur.cost + edge.cost;
            // 같은 거리도 넣으면 0비용 사이클에서 반복 가능
            if (next >= dist[edge.vertex]) continue;
            dist[edge.vertex] = next;
            Node candidate = new Node(edge.vertex, next);
            // 거리 d는 앞, d+1은 뒤로 보내 작은 거리부터 처리
            if (edge.cost == 0) deque.addFirst(candidate);
            else deque.addLast(candidate);
        }
    }
    return dist;
}
```

**예시**

```text
0 ─1→ 1       Deque(정점:거리)
└─0→ 2 ─0→ 1  [2:0, 1:1] → [1:0, 1:1]
비용 0 → 앞(d), 비용 1 → 뒤(d+1)
1:0으로 개선 → 옛 1:1은 버림
```

**주의** 비용이 0/1 외의 값이면 Dijkstra 등을 사용한다. O(V+E) 시간·공간.

[[BFS]] · [[Dijkstra]]
