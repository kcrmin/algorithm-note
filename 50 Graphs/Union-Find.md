---
type: template
tags: ["Graphs"]
aliases: ["Disjoint Set Union", "DSU"]
category: connectivity-ordering
categories: [connectivity-ordering]
---

# Union-Find

```java
static int[] parent, size;

static int find(int vertex) {
    if (parent[vertex] == vertex) return vertex;
    return parent[vertex] = find(parent[vertex]); // 대표에 바로 연결해 다음 탐색 경로 압축
}
static boolean union(int a, int b) {
    int rootA = find(a), rootB = find(b);
    if (rootA == rootB) return false;
    if (size[rootA] < size[rootB]) { // 작은 집합을 큰 집합 아래 붙여 깊이 억제
        int t = rootA; rootA = rootB; rootB = t;
    }
    parent[rootB] = rootA;
    size[rootA] += size[rootB];
    return true;
}
```

`int[] parent, size`를 `parent[i]=i`·`size[i]=1`로 초기화; 같은 find값=같은 집합, 삭제·분리 불가. 연산당 분할 상환 O(α(V)).

**분류** [[00 Maps/Connectivity and Ordering|Connectivity and Ordering]]
