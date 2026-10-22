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

static int find(int vertex) { // find: 대표를 찾으며 경로 압축
    if (parent[vertex] == vertex) return vertex;
    // compress: 다음 조회는 대표로 바로 이동
    return parent[vertex] = find(parent[vertex]);
}
// union: 서로 다른 두 집합 합치기
static boolean union(int a, int b) {
    int rootA = find(a), rootB = find(b);
    // skip: 이미 같은 대표면 변화 없음
    if (rootA == rootB) return false;
    // attach: 작은 집합을 큰 집합 아래에 붙이기
    if (size[rootA] < size[rootB]) {
        int t = rootA; rootA = rootB; rootB = t;
    }
    parent[rootB] = rootA;
    size[rootA] += size[rootB];
    return true;
}
```

`int[] parent, size`를 `parent[i]=i`·`size[i]=1`로 초기화; 같은 find값=같은 집합, 삭제·분리 불가. 연산당 분할 상환 O(α(V)).

**분류** [[00 Maps/Connectivity and Ordering|Connectivity and Ordering]]
