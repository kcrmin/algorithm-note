---
type: template
tags: ["Graphs"]
aliases: ["Residual Graph", "Min Cut"]
category: flow-matching
categories: [flow-matching]
---

# Max Flow

```java
static List<Edge>[] graph;

static class Edge {
    int to, reverse; long remaining; // 짝 역간선과 잔여 용량을 저장해 전송 취소 가능
    Edge(int t, int r, long c) { to = t; reverse = r; remaining = c; }
}
static void addEdge(int from, int to, long capacity) {
    int forwardIndex = graph[from].size();
    int reverseIndex = graph[to].size();
    if (from == to) reverseIndex++; // 자기 루프는 같은 목록에 정방향이 먼저 들어감
    graph[from].add(new Edge(to, reverseIndex, capacity));
    graph[to].add(new Edge(from, forwardIndex, 0)); // 아직 보낸 유량이 없어 취소 가능량도 0
}
```

빈 `List<Edge>[] graph`, capacity≥0·총유량 long 범위; [[Edmonds-Karp]] 또는 [[Dinic]]에서 실행(source≠sink). O(V+E) 공간.

**분류** [[00 Maps/Flow and Matching|Flow and Matching]]
