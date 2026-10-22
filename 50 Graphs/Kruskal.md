---
type: template
tags: ["Graphs"]
aliases: ["Minimum Spanning Tree", "MST"]
category: connectivity-ordering
categories: [connectivity-ordering, greedy]
---

# Kruskal

```text
    1        2
A ───── B ───── C
└────── 5 ──────┘

비용순: A-B(1) ✓ → B-C(2) ✓ → A-C(5) ✗
        다른 집합   다른 집합    이미 연결

모든 정점 연결, 총비용 3
```

| Step | Pseudocode |
|---|---|
| Sort | // 간선을 비용이 작은 순서로 정렬한다. |
| Check | // 간선 양 끝이 이미 같은 집합이면 사이클이므로 버린다. |
| Take | // 서로 다른 집합이면 합치고 비용과 선택 수를 늘린다. |
| Finish | // V-1개를 고르면 완료. 끝까지 부족하면 전체 연결은 불가능하다. |

## Java

```java
static long kruskal(int n, int[][] edges) {
    Arrays.sort(edges, Comparator.comparingInt(edge -> edge[2]));
    long total = 0;
    int selected = 0;

    for (int[] edge : edges) {
        if (!union(edge[0], edge[1])) {
            continue;
        }

        total += edge[2];
        selected++;
        if (selected == n - 1) {
            break;
        }
    }

    if (selected != n - 1) {
        throw new IllegalStateException("disconnected");
    }

    return total;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| 무방향 최소 신장 트리 | 모든 정점을 연결하는 총비용을 최소화한다. 시작점에서의 최단 경로와 목적이 다르다. |
| 왜 가장 싼 연결인가 | 현재 집합을 바깥과 잇는 최소 간선은 최적 트리의 더 비싼 연결과 교체할 수 있다. |
| Union-Find 초기화 | parent[i]=i, size[i]=1로 준비한다. 이전 계산의 집합을 재사용하면 안 된다. |
| 음수 간선 / 끊긴 그래프 | 음수도 허용한다. 이 함수는 끊기면 예외를 던진다. |

n≥1, 간선 `{u,v,cost}`. Union-Find의 union 필요. 입력 간선 배열을 정렬하며 합은 long. **O(E log E+V)**.

**분류** [[00 Maps/Connectivity and Ordering|Connectivity and Ordering]]

**필요 코드** [[50 Graphs/Union-Find|Union-Find]]
