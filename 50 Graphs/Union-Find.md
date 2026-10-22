---
type: template
tags: ["Graphs"]
aliases: ["Disjoint Set Union", "DSU"]
category: connectivity-ordering
categories: [connectivity-ordering]
---

# Union-Find

```text
처음: {0} {1} {2} {3}
union(0,1) → {0,1} {2} {3}
union(2,3) → {0,1} {2,3}
union(1,3) → {0,1,2,3}

find: 정점 → 부모 → 대표
압축: 정점 ─────────→ 대표
```

| Step | Pseudocode |
|---|---|
| Find | // 부모를 따라 집합의 대표까지 올라간다. |
| Compress | // 지나온 정점이 다음에는 대표로 바로 가도록 부모를 고친다. |
| Compare | // 두 대표가 같으면 이미 같은 집합이므로 합치지 않는다. |
| Union | // 작은 집합의 대표를 큰 집합의 대표 밑에 붙이고 크기를 더한다. |

## Java

```java
static int[] parent, size;

static int find(int vertex) {
    if (parent[vertex] == vertex) {
        return vertex;
    }

    // 대표를 반환하면서 경로도 압축한다.
    return parent[vertex] = find(parent[vertex]);
}

static boolean union(int a, int b) {
    int rootA = find(a), rootB = find(b);
    if (rootA == rootB) {
        return false;
    }

    if (size[rootA] < size[rootB]) {
        int t = rootA;
        rootA = rootB;
        rootB = t;
    }

    parent[rootB] = rootA;
    size[rootA] += size[rootB];
    return true;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| 대표끼리 연결 | 일반 정점의 부모만 바꾸면 집합 일부만 옮길 수 있다. 먼저 find를 수행한다. |
| `parent[i]=i`, `size[i]=1` | 초기에는 각 정점이 크기 1인 독립 집합이다. |
| 작은 집합 → 큰 집합 | 트리가 깊어지는 것을 제한한다. size는 대표에서만 의미가 있다. |
| 같은 대표면 false | 이미 연결된 둘을 잇는 간선은 사이클을 만든다. Kruskal에서 이 반환값을 쓴다. |
| 삭제·분리 | 합친 집합을 이 코드로 다시 나눌 수는 없다. |

find값이 같으면 같은 집합. 초기화 **O(V)**, 연산당 상환 **O(α(V))**.

**분류** [[00 Maps/Connectivity and Ordering|Connectivity and Ordering]]
