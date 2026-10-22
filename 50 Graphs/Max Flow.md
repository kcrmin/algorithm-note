---
type: template
tags: ["Graphs"]
aliases: ["Residual Graph", "Min Cut"]
---

# Max Flow

```java
// graph는 빈 List<Edge>[]로 준비. 용량은 0 이상.
// remaining은 원래 용량이 아니라 더 보낼 수 있는 양이다.
// 전송 f마다 정방향 -= f, 짝 역방향 += f. 역방향은 이전 선택을 취소하는 권리.
static class Edge {
    // reverse로 짝 간선을 즉시 찾아 양쪽 용량을 함께 갱신
    int to, reverse; long remaining;
    Edge(int t, int r, long c) { to = t; reverse = r; remaining = c; }
}
static void addEdge(int from, int to, long capacity) {
    int forwardIndex = graph[from].size();
    int reverseIndex = graph[to].size();
    // 자기 루프는 같은 목록에 정방향이 먼저 들어가므로 한 칸 보정
    if (from == to) reverseIndex++;
    graph[from].add(new Edge(to, reverseIndex, capacity));
    // 아직 보낸 유량이 없으므로 처음 취소 가능량은 0
    graph[to].add(new Edge(from, forwardIndex, 0));
}
```

**예시**

```text
u ──용량5──→ v        2를 보낸 뒤 잔여 용량
u ──3─────→ v
u ←─────2── v          역방향 2 = 이전 선택을 취소할 권리
양수 잔여 간선으로 source→sink 길이 없으면 최대 유량
```

**주의** 실행은 Edmonds-Karp 또는 Dinic 카드. source≠sink, 용량≥0, 총합 long 범위. 이 준비는 O(V+E) 공간.

[[Edmonds-Karp]] · [[Dinic]] · [[Bipartite Matching]]
