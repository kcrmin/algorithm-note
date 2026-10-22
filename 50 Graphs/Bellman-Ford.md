---
type: template
tags: ["Graphs"]
category: shortest-paths
categories: [shortest-paths]
---

# Bellman-Ford

```text
S ──4──→ A ──(-6)──→ B
└──────────1────────→ B

모든 간선 확인: B의 거리 1 → -2

최대 V-1라운드면 단순 경로의 답 도달
V번째에도 감소 → 닿을 수 있는 음수 사이클
```

| Step | Pseudocode |
|---|---|
| Round | // 모든 간선을 훑으며 거리를 줄이는 일을 반복한다. |
| Skip | // 시작점에서 닿지 못한 정점에서 나오는 간선은 건너뛴다. |
| Relax | // 출발 정점의 거리와 간선 비용의 합이 더 작으면 갱신한다. |
| Detect | // V번째 라운드에도 갱신되면 도달 가능한 음수 사이클이다. |
| Stop | // 한 라운드 동안 변화가 없으면 거리 계산이 끝난다. |

## Java

```java
static List<long[]> edges;
static long[] dist;
static final long INF = Long.MAX_VALUE / 4;

static boolean hasNegativeCycle(int n) {
    for (int round = 1; round <= n; round++) {
        boolean updated = false;

        for (long[] edge : edges) {
            int from = (int) edge[0], to = (int) edge[1];
            if (dist[from] == INF) {
                continue;
            }

            long next = dist[from] + edge[2];
            if (next >= dist[to]) {
                continue;
            }

            dist[to] = next;
            updated = true;
            if (round == n) {
                return true;
            }
        }

        if (!updated) {
            return false;
        }
    }

    return false;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| V-1과 V | 사이클 없는 경로는 간선이 최대 V-1개. 그 이후에도 개선된다면 음수 사이클을 이용한 것이다. |
| `dist[from] == INF` 검사 | 도달하지 못한 정점에서 거리 계산을 시작하지 않는다. |
| 도달 가능한 사이클만 | dist[source]만 0으로 시작한다. 그래프 전체의 음수 사이클 탐지는 모든 거리를 0으로 시작하는 별도 초기화다. |
| 초기 상태 | dist[source]=0, 나머지 거리=INF. true를 반환했다면 dist 전체를 유한 최단 거리로 해석하지 않는다. |

`edges`의 원소 `{from,to,cost}`. 반환: 도달 가능한 음수 사이클 여부. 거리와 계산 합은 (-INF,INF), INF=`Long.MAX_VALUE/4`. **O(VE)**.

**분류** [[00 Maps/Shortest Paths|Shortest Paths]]
