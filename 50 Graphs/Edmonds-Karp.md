---
type: template
tags: ["Graphs"]
---

# Edmonds-Karp

**언제** Max Flow를 단순하게 구현할 때, 잔여 Graph의 최단 증대 경로를 BFS로 반복 찾는다.

```text
s ─3→ a ─2→ t     병목 = min(3,2) = 2
잔여 s ─1→ a ─0→ t
역방향에는 각각 +2      total = 2
다시 BFS → 길이 없을 때 종료
```

**코드** `Max Flow`의 Edge/addEdge/graph 사용. `via[v]`는 v로 들어온 간선, `parent[v]`는 그 출발점.

```java
static long maxFlow(int source, int sink) {
    long total = 0;
    int n = graph.length;
    while (true) {
        int[] parent = new int[n];
        Arrays.fill(parent, -1);
        Edge[] via = new Edge[n];
        Queue<Integer> queue = new ArrayDeque<>();
        parent[source] = source;
        queue.offer(source);
        while (!queue.isEmpty() && parent[sink] == -1) {
            int cur = queue.poll();
            for (Edge edge : graph[cur]) {
                if (edge.remaining <= 0 || parent[edge.to] != -1) continue;
                parent[edge.to] = cur;
                via[edge.to] = edge;
                queue.offer(edge.to);
            }
        }
        if (parent[sink] == -1) return total;
        long sent = Long.MAX_VALUE;
        for (int v = sink; v != source; v = parent[v]) {
            sent = Math.min(sent, via[v].remaining); // ①
        }
        for (int v = sink; v != source; v = parent[v]) {
            Edge edge = via[v];
            edge.remaining -= sent;
            graph[v].get(edge.reverse).remaining += sent; // ②
        }
        total += sent;
    }
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 경로 용량의 최솟값 | 어느 간선의 용량도 넘지 않는 최대 전송량이다. |
| ② 역간선에 같은 양 추가 | 나중 경로에서 이번 유량을 취소·재배치할 수 있다. |
| BFS로 경로 선택 | 잔여 간선 수가 가장 짧은 경로를 골라 O(VE²)을 보장한다. |

**주의** graph의 잔여 용량을 변경한다. 새 문제는 새 Graph로 시작. O(VE²) 시간, O(V+E) 공간.

[[Max Flow]] · [[Dinic]] · [[BFS]]
