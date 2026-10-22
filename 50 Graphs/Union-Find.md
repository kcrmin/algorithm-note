---
type: template
tags: ["Graphs"]
aliases: ["Disjoint Set Union", "DSU"]
---

# Union-Find

```java
// parent[i]=i, size[i]=1로 초기화. find(a)==find(b)이면 같은 집합.
static int find(int vertex) {
    if (parent[vertex] == vertex) return vertex;
    // 중간 부모를 대표로 바꿔 다음 find의 경로를 압축
    return parent[vertex] = find(parent[vertex]);
}
static boolean union(int a, int b) {
    int rootA = find(a), rootB = find(b);
    if (rootA == rootB) return false; // 이미 연결된 집합은 다시 합칠 필요 없음
    if (size[rootA] < size[rootB]) { // 작은 집합을 큰 집합 아래 붙여 깊이를 억제
        int t = rootA; rootA = rootB; rootB = t;
    }
    parent[rootB] = rootA; // 임의 원소가 아닌 대표끼리 연결해야 집합 전체가 합쳐짐
    size[rootA] += size[rootB];
    return true;
}
```

**예시**

```text
parent: 0←1←2       find(2)
        0←1          2→1→0을 찾은 뒤 2→0으로 압축
        ↑2
union(0,3): 대표끼리 연결 → {0,1,2,3}
```

**주의** 간선 삭제·집합 분리는 지원하지 않는다. 연산당 분할 상환 O(α(V)), 공간 O(V).

[[Kruskal]] · [[DFS and Graphs]]
