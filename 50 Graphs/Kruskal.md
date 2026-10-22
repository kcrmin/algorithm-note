---
type: template
tags: ["Graphs"]
aliases: ["Minimum Spanning Tree", "MST"]
category: connectivity-ordering
categories: [connectivity-ordering, greedy]
---

# Kruskal

```java
static long kruskal(int n, int[][] edges) {
    // sort: 가중치가 작은 간선부터 확인
    Arrays.sort(edges, Comparator.comparingInt(edge -> edge[2]));
    long total = 0;
    int selected = 0;
    for (int[] edge : edges) {
        // skip: 같은 집합을 잇는 간선은 사이클이라 제외
        if (!union(edge[0], edge[1])) continue;
        // take: 서로 다른 집합을 잇는 간선 선택
        total += edge[2];
        selected++;
        if (selected == n - 1) break;
    }
    // check: V-1개를 못 고르면 연결 실패
    if (selected != n - 1) throw new IllegalStateException("disconnected");
    return total;
}
```

무방향 간선 `{u,v,cost}`; [[Union-Find]]의 `union`, `parent[i]=i`·`size[i]=1` 준비; 끊기면 예외, 간선 배열 정렬. n≥1. O(E log E+V).

**분류** [[00 Maps/Connectivity and Ordering|Connectivity and Ordering]]

**필요 코드** [[50 Graphs/Union-Find|Union-Find]]
