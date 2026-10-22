---
type: template
tags: ["Graphs"]
category: flow-matching
categories: [flow-matching]
---

# Edmonds-Karp

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
        if (parent[sink] == -1) return total; // 증대 경로가 없으면 최대 유량
        long sent = Long.MAX_VALUE;
        for (int v = sink; v != source; v = parent[v]) {
            sent = Math.min(sent, via[v].remaining); // 경로의 가장 좁은 용량이 전송량
        }
        for (int v = sink; v != source; v = parent[v]) {
            Edge edge = via[v];
            edge.remaining -= sent;
            graph[v].get(edge.reverse).remaining += sent; // 나중에 이 전송을 취소할 수 있는 양
        }
        total += sent;
    }
}
```

[[Max Flow]]의 `Edge`·`graph` 사용, source≠sink; 잔여 용량을 변경하므로 새 계산은 새 그래프에서 시작. O(VE²).

**분류** [[00 Maps/Flow and Matching|Flow and Matching]]

**필요 코드** [[50 Graphs/Max Flow|Max Flow]]

**비교** [[50 Graphs/Dinic|Dinic]]
