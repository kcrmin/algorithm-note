---
type: data-structure
tags:
  - disjoint-set
  - union-find
  - dsu
  - graph
language: java
status: in-review
source: []
---

# Union Find (Disjoint Set Union, DSU)
> **한 줄 요약:** 원소들의 집합을 관리하며, 같은 집합 여부 확인과 집합 합치기를 빠르게 처리하는 자료구조

## Core Concept
* **When to use:**
  - 간선이 추가되면서 연결 관계를 관리해야 할 때
  - 두 노드가 같은 컴포넌트인지 자주 물어볼 때
  - 사이클 판별, Kruskal MST
* **Key Point:**
  - 각 원소는 하나의 집합에 속한다 (대표 노드로 관리)
  - find(x): x가 속한 집합의 대표 노드 반환 (경로 압축)
  - union(a, b): a와 b가 속한 집합을 합친다 (union by size/rank)

## API & State Change (if applicable)
| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Init | `parent[i] = i; size[i] = 1;` | 초기화 | 각 원소가 자기 집합 |
| Find | `find(x)` | root 반환 | 대표 노드 찾기(경로 압축) |
| Union | `union(a,b)` | 상태 변경 | 두 집합 합치기 |
| Check | `find(a) == find(b)` | true/false | 같은 집합 여부 |

## Patterns
### Pattern A: 표준 DSU (Path Compression + Union by Size)
- 가장 기본으로 쓰는 구현
```java
class DSU {
    int[] parent, size;

    DSU(int n) {
        parent = new int[n];
        size = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }

    int find(int x) {
        if (parent[x] == x) return x;
        parent[x] = find(parent[x]); // path compression
        return parent[x];
    }

    boolean union(int a, int b) {
        int ra = find(a);
        int rb = find(b);
        if (ra == rb) return false; // already connected

        // union by size (small -> large)
        if (size[ra] < size[rb]) {
            int tmp = ra; ra = rb; rb = tmp;
        }
        parent[rb] = ra;
        size[ra] += size[rb];
        return true;
    }
}
```

### Pattern B: 사이클 판별 (간선 추가)
- `union(u, v)`가 실패하면 사이클
```java
if (!dsu.union(u, v)) {
    // u-v 추가가 사이클을 만든다
}
```

## Pitfalls & Tips
- 0/1-index 혼동 주의 (입력이 1-index면 -1 처리)
- find를 union/check 전에 항상 호출해야 한다
- 간선 삭제/되돌리기는 기본 DSU로 처리하기 어렵다
