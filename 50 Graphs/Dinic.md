---
type: template
tags: ["Graphs"]
category: flow-matching
categories: [flow-matching]
---

# Dinic

```text
BFS로 만든 층:   0     1     2
                 S → A → T
                  ↘ B ↗

같은 층 그래프에서:
S→A→T 전송 → S→B→T 전송 → 더 못 보내면
잔여 그래프를 BFS해 새 층 구성
```

| Step | Pseudocode |
|---|---|
| Build | // 잔여 간선 BFS로 source에서의 층을 만든다. |
| Send | // 정확히 다음 층으로 가는 간선만 DFS하며 보낸다. |
| Advance | // 실패한 간선은 커서를 넘긴다. 성공한 간선은 남은 용량을 다시 쓸 수 있다. |
| Rebuild | // 현재 층에서 더 못 보내면 새 층을 만들고 커서를 초기화한다. |
| Finish | // BFS로 sink에 닿지 못하면 누적 유량이 답이다. |

## Java

```java
static int[] level, next;

static boolean buildLevels(int source, int sink) {
    Arrays.fill(level, -1);
    Queue<Integer> queue = new ArrayDeque<>();
    level[source] = 0;
    queue.offer(source);

    while (!queue.isEmpty()) {
        int cur = queue.poll();

        for (Edge edge : graph[cur]) {
            if (edge.remaining <= 0 || level[edge.to] != -1) {
                continue;
            }

            level[edge.to] = level[cur] + 1;
            queue.offer(edge.to);
        }
    }

    return level[sink] != -1;
}

static long send(int cur, int sink, long limit) {
    if (cur == sink) {
        return limit;
    }

    // 실패한 간선은 넘기고, 성공한 간선에서는 즉시 반환한다.
    for (; next[cur] < graph[cur].size(); next[cur]++) {
        Edge edge = graph[cur].get(next[cur]);
        if (edge.remaining <= 0 || level[edge.to] != level[cur] + 1) {
            continue;
        }

        long sent = send(edge.to, sink, Math.min(limit, edge.remaining));
        if (sent == 0) {
            continue;
        }

        edge.remaining -= sent;
        graph[edge.to].get(edge.reverse).remaining += sent;
        return sent;
    }

    return 0;
}

static long maxFlow(int source, int sink) {
    long total = 0;

    while (buildLevels(source, sink)) {
        // 새 층에서는 간선의 가능성을 처음부터 다시 본다.
        Arrays.fill(next, 0);
        long sent;

        while ((sent = send(source, sink, Long.MAX_VALUE)) > 0) {
            total += sent;
        }
    }

    return total;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| `level[to] = level[cur] + 1` | 층이 증가하는 방향만 써서 DFS가 순환하지 않는다. |
| `next[cur]` 커서 | 현재 층에서 실패한 간선을 반복 탐색하지 않는다. 성공 즉시 반환하면 그 간선을 다음 호출에서도 재사용한다. |
| 새 층 → 커서 0 | 잔여 그래프가 바뀌면 전에 실패한 간선도 새 경로에 쓰일 수 있다. |
| 정방향 − / 역방향 + | 부분 전송을 취소할 수 있어야 다른 경로로 유량을 재배치할 수 있다. |
| 초기 상태와 재귀 | level·next는 길이 V. source≠sink, 새 계산은 새 잔여 그래프. 긴 경로는 재귀 스택에 주의한다. |

아래 Max Flow의 Edge·graph 필요. 반환: 최대 유량. 일반 용량 그래프에서 **O(V²E)**.

**분류** [[00 Maps/Flow and Matching|Flow and Matching]]

**필요 코드** [[50 Graphs/Max Flow|Max Flow]]
