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
    Arrays.sort(edges, Comparator.comparingInt(edge -> edge[2])); // 서로 다른 집합을 가장 싼 간선부터 연결
    long total = 0;
    int selected = 0;
    for (int[] edge : edges) {
        if (!union(edge[0], edge[1])) continue; // 이미 연결된 두 점을 잇면 사이클
        total += edge[2];
        selected++;
        if (selected == n - 1) break;
    }
    if (selected != n - 1) throw new IllegalStateException("disconnected");
    return total;
}
```

무방향 간선 `{u,v,cost}`; [[Union-Find]]의 `union`, `parent[i]=i`·`size[i]=1` 준비; 끊기면 예외, 간선 배열 정렬. n≥1. O(E log E+V).

**분류** [[00 Maps/Connectivity and Ordering|Connectivity and Ordering]]

**필요 코드** [[50 Graphs/Union-Find|Union-Find]]
