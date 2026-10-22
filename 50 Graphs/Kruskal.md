---
type: template
tags: ["Graphs"]
aliases: ["Minimum Spanning Tree", "MST"]
---

# Kruskal

**언제** 무방향 가중 Graph의 모든 정점을 연결하는 최소 비용 Tree를 구한다.

```text
간선(비용순): 0—1(1), 1—2(2), 0—2(3), 2—3(4)
              ✓        ✓        ×        ✓
0—2는 이미 연결되어 사이클 → 버림
선택 3개 = V-1 → 비용 1+2+4 = 7
```

**코드** `Union-Find`의 find/union과 새 parent/size 사용. `edges`는 `{u,v,cost}` 배열.

```java
static long kruskal(int n, int[][] edges) {
    Arrays.sort(edges, Comparator.comparingInt(edge -> edge[2]));
    long total = 0;
    int selected = 0;
    for (int[] edge : edges) {
        if (!union(edge[0], edge[1])) continue; // ①
        total += edge[2];
        selected++;
        if (selected == n - 1) break;
    }
    if (selected != n - 1) throw new IllegalStateException("disconnected");
    return total;
}
```

| 암기 포인트 | 이유 |
|---|---|
| 싼 간선부터 선택 | 현재 덩어리들을 잇는 가장 싼 간선은 MST에 포함시켜도 된다. |
| ① 다른 집합일 때만 선택 | 같은 집합이면 이미 경로가 있어 새 간선은 사이클을 만든다. |
| 선택 수 = V-1 | 모든 정점이 연결되었는지 비용 합과 별개로 확인한다. |

**주의** 끊긴 Graph는 위 코드에서 예외. 음수 비용도 가능. O(E log E+V) 시간, O(V+E) 공간.

[[Union-Find]] · [[Greedy]] · [[Heap]]
