---
type: template
tags: ["Graphs"]
aliases: ["Disjoint Set Union", "DSU"]
---

# Union-Find

**언제** 집합을 합치는 연산과 두 원소가 같은 집합인지 확인하는 연산을 반복한다.

```text
parent: 0←1←2       find(2)
        0←1          2→1→0을 찾은 뒤 2→0으로 압축
        ↑2
union(0,3): 대표끼리 연결 → {0,1,2,3}
```

**코드** `parent[i]=i`, `size[i]=1`로 초기화. `find(a)==find(b)`면 같은 집합.

```java
static int find(int vertex) {
    if (parent[vertex] == vertex) return vertex;
    return parent[vertex] = find(parent[vertex]); // ①
}
static boolean union(int a, int b) {
    int rootA = find(a), rootB = find(b);
    if (rootA == rootB) return false;
    if (size[rootA] < size[rootB]) {
        int t = rootA; rootA = rootB; rootB = t;
    }
    parent[rootB] = rootA; // ②
    size[rootA] += size[rootB];
    return true;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 경로 압축 | 다음 find에서는 중간 부모를 건너뛰고 대표로 바로 간다. |
| ② 작은 집합을 큰 집합 밑에 | 부모 Tree가 깊어지는 것을 억제한다. |
| 대표가 같으면 false | 이미 한 집합이므로 연결 정보가 변하지 않는다. |

**주의** 간선 삭제·집합 분리는 지원하지 않는다. 연산당 분할 상환 O(α(V)), 공간 O(V).

[[Kruskal]] · [[DFS and Graphs]]
