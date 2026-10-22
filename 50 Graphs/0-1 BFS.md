---
type: template
tags: ["Graphs"]
category: shortest-paths
categories: [shortest-paths]
---

# 0-1 BFS

```text
S ──1──→ A
│        ↑
0        0
↓        │
B ───────┘       A까지 최소 비용: 1 → 0

Deque 앞 ← 비용 0으로 이어진 후보
Deque 뒤 ← 비용 1로 이어진 후보
```

| Step | Pseudocode |
|---|---|
| Pop | // Deque 앞에서 누적 거리가 작은 후보를 꺼낸다. |
| Skip | // 현재 거리보다 낡은 후보는 건너뛴다. |
| Relax | // 더 짧은 거리를 찾으면 기록한다. |
| Place | // 추가 비용 0은 앞, 1은 뒤에 넣어 거리 순서를 유지한다. |

## Java

```java
static long[] zeroOneBfs(List<int[]>[] graph, int start) {
    long INF = Long.MAX_VALUE / 4;
    long[] dist = new long[graph.length];
    Arrays.fill(dist, INF);
    Deque<long[]> deque = new ArrayDeque<>();
    dist[start] = 0;
    deque.addFirst(new long[]{start, 0});

    while (!deque.isEmpty()) {
        long[] cur = deque.removeFirst();
        int v = (int) cur[0];
        if (cur[1] != dist[v]) {
            continue;
        }

        for (int[] edge : graph[v]) {
            int to = edge[0];
            long next = cur[1] + edge[1];

            // 첫 방문 여부가 아니라 거리 개선 여부를 본다.
            if (next >= dist[to]) {
                continue;
            }

            dist[to] = next;
            long[] state = {to, next};
            if (edge[1] == 0) {
                deque.addFirst(state);
            } else {
                deque.addLast(state);
            }
        }
    }

    return dist;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| 가중치 0 또는 1 | 현재 거리의 후보와 한 단계 먼 후보만 구분하면 되므로 힙 없이 Deque를 쓴다. |
| 0은 앞 / 1은 뒤 | 비용 0인 이웃은 아직 거리도 같다. 비용 1인 이웃보다 먼저 처리한다. |
| 첫 방문만 허용하면 실패 | 비용 1로 먼저 찾은 정점을 나중에 비용 0 경로로 개선할 수 있다. |
| 엄격히 더 짧을 때 | 같은 거리의 재등록은 0 비용 사이클에서 반복을 만든다. |

`graph[v]`의 간선 `{to, weight}`; 상태 `{vertex, distance}`. 도달 불가=`Long.MAX_VALUE/4`. **O(V+E)**.

**분류** [[00 Maps/Shortest Paths|Shortest Paths]]

**비교** [[30 Search/BFS|BFS]]
