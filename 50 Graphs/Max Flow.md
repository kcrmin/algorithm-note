---
type: template
tags: ["Graphs"]
aliases: ["Residual Graph", "Min Cut"]
---

# Max Flow

```java
static class Edge {
    int to, reverse; long remaining;
    Edge(int t, int r, long c) { to = t; reverse = r; remaining = c; }
}
static void addEdge(int from, int to, long capacity) {
    int forwardIndex = graph[from].size();
    int reverseIndex = graph[to].size();
    if (from == to) reverseIndex++;
    graph[from].add(new Edge(to, reverseIndex, capacity));
    graph[to].add(new Edge(from, forwardIndex, 0)); // ①
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 역간선도 반드시 생성 | 다른 경로가 필요해지면 이미 보낸 유량 일부를 되돌릴 수 있다. |
| 보낸 양 f → 정방향 -f / 역방향 +f | 잔여 용량은 더 보낼 수 있는 양이지 원래 용량이 아니다. |
| 더 보낼 길이 없음 = 종료 | 도달 집합과 나머지 사이의 Cut이 막히며 최대 유량=최소 Cut이다. |

**예시**

```text
u ──용량5──→ v        2를 보낸 뒤 잔여 용량
u ──3─────→ v
u ←─────2── v          역방향 2 = 이전 선택을 취소할 권리
양수 잔여 간선으로 source→sink 길이 없으면 최대 유량
```

**적용** 방향별 용량을 지키며 source에서 sink로 보낼 수 있는 총량을 최대화한다.

**준비** 공통 준비: `List<Edge>[] graph`를 빈 목록으로 만든다. 간선 입력마다 `addEdge(u,v,capacity)`.

**주의** 실행은 Edmonds-Karp 또는 Dinic 카드. source≠sink, 용량≥0, 총합 long 범위. 이 준비는 O(V+E) 공간.

[[Edmonds-Karp]] · [[Dinic]] · [[Bipartite Matching]]
