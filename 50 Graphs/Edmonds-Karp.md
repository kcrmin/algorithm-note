---
type: template
tags: ["Graphs"]
category: flow-matching
categories: [flow-matching]
---

# Edmonds-Karp

```text
S ──5──→ A ──3──→ T

BFS로 경로 찾기: S → A → T
보낼 양: min(5,3) = 3

잔여 용량: S ──2──→ A ──0──→ T
역간선도 +3 → 다음 BFS에서 경로 재탐색
```

| Step | Pseudocode |
|---|---|
| Find | // 양의 잔여 용량만 따라 BFS하고 부모와 사용 간선을 저장한다. |
| Bottleneck | // 도착점에서 부모를 거슬러 올라가며 가장 작은 잔여 용량을 찾는다. |
| Send | // 경로의 정방향에서는 보내는 양을 빼고 역방향에는 더한다. |
| Repeat | // 다시 BFS한다. 도착점에 닿지 못하면 누적 유량이 답이다. |

## Java

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
                if (edge.remaining <= 0 || parent[edge.to] != -1) {
                    continue;
                }

                parent[edge.to] = cur;
                via[edge.to] = edge;
                queue.offer(edge.to);
            }
        }

        if (parent[sink] == -1) {
            return total;
        }

        long sent = Long.MAX_VALUE;

        for (int v = sink; v != source; v = parent[v]) {
            sent = Math.min(sent, via[v].remaining);
        }

        for (int v = sink; v != source; v = parent[v]) {
            Edge edge = via[v];
            edge.remaining -= sent;

            // 앞서 보낸 유량을 취소할 수 있게 역방향을 늘린다.
            graph[v].get(edge.reverse).remaining += sent;
        }

        total += sent;
    }
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| `parent[source]=source` | 시작점을 방문 처리해 역간선으로 다시 들어오는 것을 막는다. |
| 부모와 간선 모두 저장 | 같은 두 정점 사이에 여러 간선이 있을 수 있어 실제 사용 간선도 기억한다. |
| 경로의 최소 용량 | 모든 간선을 통과해야 하므로 가장 좁은 간선이 한 번에 보낼 양을 제한한다. |
| 정방향 − / 역방향 + | 이전 경로를 취소하고 더 좋은 경로로 재배치할 여지를 남긴다. |
| source≠sink / 새 계산 | 같으면 빈 경로를 무한히 증대한다. 잔여 용량을 소모하므로 새 계산은 새 그래프로 시작한다. |

아래 Max Flow의 Edge·graph 필요. 반환: 최대 유량. **O(VE²)**.

**분류** [[00 Maps/Flow and Matching|Flow and Matching]]

**필요 코드** [[50 Graphs/Max Flow|Max Flow]]

**비교** [[50 Graphs/Dinic|Dinic]]
