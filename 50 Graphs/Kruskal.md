---
type: template
tags: ["Graphs"]
aliases: ["Minimum Spanning Tree", "MST"]
---

# Kruskal

```java
// Union-Find 카드의 union 사용; parent[i]=i, size[i]=1로 매번 초기화.
// edges의 각 원소는 {u,v,cost}. 무방향 Graph이며 음수 비용도 가능.
static long kruskal(int n, int[][] edges) {
    // 서로 다른 덩어리를 잇는 가장 싼 간선부터 선택
    Arrays.sort(edges, Comparator.comparingInt(edge -> edge[2]));
    long total = 0;
    int selected = 0;
    for (int[] edge : edges) {
        // 이미 같은 집합이면 경로가 있으므로 사이클이 생김
        if (!union(edge[0], edge[1])) continue;
        total += edge[2];
        selected++;
        if (selected == n - 1) break; // Tree는 정점 V개를 간선 V-1개로 연결
    }
    // 비용 합만으로는 전체 연결 성공을 알 수 없음
    if (selected != n - 1) throw new IllegalStateException("disconnected");
    return total;
}
```

**예시**

```text
간선(비용순): 0—1(1), 1—2(2), 0—2(3), 2—3(4)
              ✓        ✓        ×        ✓
0—2는 이미 연결되어 사이클 → 버림
선택 3개 = V-1 → 비용 1+2+4 = 7
```

**주의** 끊긴 Graph는 위 코드에서 예외. 음수 비용도 가능. O(E log E+V) 시간, O(V+E) 공간.

[[Union-Find]] · [[Greedy]] · [[Heap]]
