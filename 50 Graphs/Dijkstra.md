---
type: template
tags: ["Graphs"]
category: shortest-paths
categories: [shortest-paths]
---

# Dijkstra

```text
        7
    S ─────→ A
    │        ↑
  2 │        │ 1
    ↓        │
    B ───────┘

거리: S=0 → B=2 → A=3   (A의 후보 7 → 3)
Heap: 가장 짧은 후보부터 꺼내 주변 거리를 줄임
```

| Step | Pseudocode |
|---|---|
| Seed | // 시작 거리만 0, 나머지는 무한대로 두고 시작 후보를 넣는다. |
| Pop | // 현재 후보 중 누적 거리가 가장 작은 정점을 꺼낸다. |
| Skip | // 더 짧은 경로가 발견되어 낡아진 후보면 버린다. |
| Relax | // 현재 정점을 거친 거리가 더 짧으면 기록하고 새 후보를 넣는다. |
| Answer | // 시작점에서 모든 정점까지의 최단 거리 배열을 반환한다. |

## Java

```java
static long[] dijkstra(List<int[]>[] graph, int start) {
    long INF = Long.MAX_VALUE / 4;
    long[] dist = new long[graph.length];
    Arrays.fill(dist, INF);
    PriorityQueue<long[]> pq = new PriorityQueue<>(
            Comparator.comparingLong(a -> a[1]));
    dist[start] = 0;
    pq.offer(new long[]{start, 0});

    while (!pq.isEmpty()) {
        long[] cur = pq.poll();
        int v = (int) cur[0];

        // 더 짧은 경로가 나온 뒤 남아 있는 옛 후보.
        if (cur[1] != dist[v]) {
            continue;
        }

        for (int[] edge : graph[v]) {
            int to = edge[0];
            long next = cur[1] + edge[1];

            // 같은 거리는 다시 등록하지 않는다.
            if (next >= dist[to]) {
                continue;
            }

            dist[to] = next;
            pq.offer(new long[]{to, next});
        }
    }

    return dist;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| 음수 간선 금지 | 꺼낸 최소 거리보다 나중에 더 싸지는 역전이 없어야 한다. 음수 간선은 이 전제를 깬다. |
| `{vertex, distance}` / `a[1]` | 힙은 정점 번호가 아니라 누적 거리로 정렬한다. |
| 옛 후보 제거 | A의 7과 3이 힙에 함께 남을 수 있다. 현재 dist와 다른 7은 건너뛴다. |
| 엄격히 짧을 때만 갱신 | 같은 거리도 넣으면 비용 0인 사이클에서 같은 후보가 끝없이 늘 수 있다. |

`graph[v]`의 간선 `{to, weight}`, weight≥0인 int. 반환: `long[] dist`, 도달 불가=`Long.MAX_VALUE/4`; 거리 합은 INF 미만. **O(V+E log(E+2))**.

**분류** [[00 Maps/Shortest Paths|Shortest Paths]]

**비교** [[50 Graphs/0-1 BFS|0-1 BFS]] · [[50 Graphs/Bellman-Ford|Bellman-Ford]]
