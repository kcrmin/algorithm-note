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

// Edge: 잔여 용량과 짝 역간선 위치 저장
static class Edge {
    int to, reverse; long remaining;
    Edge(int t, int r, long c) { to = t; reverse = r; remaining = c; }
}
// addEdge: 용량 간선과 취소용 역간선을 함께 추가
static void addEdge(int from, int to, long capacity) {
    int forwardIndex = graph[from].size();
    int reverseIndex = graph[to].size();
    // pair: 자기 루프면 정방향 뒤의 역간선 가리키기
    if (from == to) reverseIndex++;
    graph[from].add(new Edge(to, reverseIndex, capacity));
    // reverse: 처음에는 취소할 전송이 없어 용량 0
    graph[to].add(new Edge(from, forwardIndex, 0));
}
```

빈 `List<Edge>[] graph`, capacity≥0·총유량 long 범위; [[Edmonds-Karp]] 또는 [[Dinic]]에서 실행(source≠sink). O(V+E) 공간.

**분류** [[00 Maps/Flow and Matching|Flow and Matching]]
