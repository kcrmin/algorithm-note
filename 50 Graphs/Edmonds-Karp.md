---
type: template
tags: ["Graphs"]
---

# Edmonds-Karp

```java
// Max Flow 카드의 Edge/addEdge/graph 사용. source≠sink, remaining=잔여 용량.
// parent[v]는 이전 정점, via[v]는 v로 들어온 실제 간선: BFS 경로를 뒤로 복원한다.
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
        // 잔여 간선 수가 가장 짧은 증대 경로를 BFS로 찾음
        while (!queue.isEmpty() && parent[sink] == -1) {
            int cur = queue.poll();
            for (Edge edge : graph[cur]) {
                if (edge.remaining <= 0 || parent[edge.to] != -1) continue;
                parent[edge.to] = cur;
                via[edge.to] = edge;
                queue.offer(edge.to);
            }
        }
        // 더 보낼 경로가 없으면 현재 유량이 최대
        if (parent[sink] == -1) return total;
        long sent = Long.MAX_VALUE;
        for (int v = sink; v != source; v = parent[v]) {
            // 경로 중 가장 좁은 용량이 이번 전송량
            sent = Math.min(sent, via[v].remaining);
        }
        for (int v = sink; v != source; v = parent[v]) {
            Edge edge = via[v];
            edge.remaining -= sent;
            // 나중 경로에서 이번 전송을 취소·재배치할 수 있게 함
            graph[v].get(edge.reverse).remaining += sent;
        }
        total += sent;
    }
}
```

**예시**

```text
s ─3→ a ─2→ t     병목 = min(3,2) = 2
잔여 s ─1→ a ─0→ t
역방향에는 각각 +2      total = 2
다시 BFS → 길이 없을 때 종료
```

**주의** graph의 잔여 용량을 변경한다. 새 문제는 새 Graph로 시작. O(VE²) 시간, O(V+E) 공간.

[[Max Flow]] · [[Dinic]] · [[BFS]]
