---
type: template
cssclasses: [algorithm-reference]
tags: [그래프]
aliases: [Union_Find, MST]
---

# 유니온 파인드와 MST

> **핵심:** 같은 집합은 대표가 같다. MST는 싼 간선부터 보며 서로 다른 집합일 때만 연결한다.

## 개념

유니온 파인드는 원소들이 속한 집합을 대표 원소로 관리하고, 두 집합을 합치는 자료구조다. Kruskal은 간선을 싼 순서로 보며 다른 집합을 잇는 간선만 골라 모든 정점을 연결하는 최소 신장 트리(MST)를 만든다.

## 적용 조건

- Union-Find: 여러 원소를 집합으로 합치고 두 원소가 같은 집합인지 반복해서 물을 때 쓴다.
- MST: **무향 가중치 그래프**의 모든 정점을 연결하는 최소 비용이 필요할 때 쓴다.
- MST는 시작점에서의 최단거리가 아니다. “전체 연결 비용”과 “특정 경로 비용”을 구분한다.
- Kruskal은 간선을 비용순으로 정렬하고 사이클을 만드는 간선을 제외한다.
- 정점은 `0 ~ n-1`, `edges[i]={정점1, 정점2, 비용}`이다. 간선 비용은 음수여도 된다.
- 정렬을 포함한 시간 `O(E log E + V)`, 공간 `O(V+E)`.
- Union-Find만 보면 크기 기준 합치기+경로 압축의 연산당 분할 상환 시간은 `O(α(V))`다.

## Java 템플릿

연결 실패는 `OptionalLong.empty()`로 표현한다. 비용이 음수일 수도 있으므로 `-1`을 실패값으로 쓰지 않는다.

```java
import java.util.*;

class KruskalTemplate {
    static class UnionFind {
        int[] parent;
        int[] size;

        UnionFind(int n) {
            parent = new int[n];
            size = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                size[i] = 1;
            }
        }

        int find(int vertex) {
            if (parent[vertex] == vertex) return vertex;
            parent[vertex] = find(parent[vertex]);
            return parent[vertex];
        }

        boolean union(int a, int b) {
            int rootA = find(a);
            int rootB = find(b);
            if (rootA == rootB) return false;

            if (size[rootA] < size[rootB]) {
                int temporary = rootA;
                rootA = rootB;
                rootB = temporary;
            }
            parent[rootB] = rootA;
            size[rootA] += size[rootB];
            return true;
        }
    }

    static OptionalLong minimumSpanningTree(int n, int[][] edges) {
        if (n <= 1) return OptionalLong.of(0);

        int[][] sorted = edges.clone();
        Arrays.sort(sorted, Comparator.comparingInt(edge -> edge[2]));
        UnionFind uf = new UnionFind(n);

        long total = 0;
        int selected = 0;
        for (int[] edge : sorted) {
            if (!uf.union(edge[0], edge[1])) continue;

            total += edge[2];
            selected++;
            if (selected == n - 1) return OptionalLong.of(total);
        }
        return OptionalLong.empty();
    }
}
```

> [!info]- 원리와 코드 설명
>
> 4개 정점의 간선이 `(0,1,1), (1,2,2), (0,2,3), (2,3,4)`이면 다음과 같다.
>
> | 비용순 간선 | 집합 판단 | 선택 수 | 합계 |
> |---|---|---:|---:|
> | `0—1:1` | 다른 집합, 합침 | 1 | 1 |
> | `1—2:2` | 다른 집합, 합침 | 2 | 3 |
> | `0—2:3` | 이미 같은 집합, 제외 | 2 | 3 |
> | `2—3:4` | 다른 집합, 합침 | 3 | 7 |
>
> `0—2`를 추가하면 `0—1—2—0` 사이클이 생긴다. 이미 연결된 두 정점 사이의 간선은 전체 연결에 필요하지 않다. N개 정점의 트리는 간선이 N−1개이므로 3개를 고르면 끝난다.
>
> **왜 싼 간선부터 고르는가?** 서로 다른 두 집합을 잇는 가장 싼 간선을 다른 연결 간선 대신 사용해도 전체 비용이 늘지 않는다. 이 연결을 확정하고 남은 집합들에 같은 선택을 반복한다.
>
> **왜 대표를 따라가는가?** `parent[2]=1`, `parent[1]=0`이면 2의 대표는 1이 아니라 0이다. `find(2)`가 대표 0까지 찾고 `parent[2]=0`으로 줄여 다음 조회를 빠르게 한다. 이것이 경로 압축이다.
>
> `size`는 대표가 가진 집합 크기다. 작은 집합의 대표를 큰 집합의 대표 아래에 붙이면 길게 늘어진 부모 사슬을 줄인다.
>
> 간선이 `(0,1,1), (2,3,2)`뿐이면 선택 수가 2에서 끝난다. 4개 전체를 연결하려면 3개가 필요하므로 결과는 빈 `OptionalLong`이다. 합계 3을 MST 비용으로 반환하면 안 된다.
>
> ### 주의점
>
> - 단순 연결 여부는 `uf.find(a) == uf.find(b)`로 검사한다.
> - `parent[a] == parent[b]`는 대표가 아닌 중간 부모끼리 비교할 수 있어 틀린다.
> - 합칠 때 원소 a/b가 아니라 **대표 rootA/rootB**의 부모와 크기를 바꾼다.
> - 간선 입력이 무향이라도 Kruskal 목록에는 한 번만 넣으면 된다. 인접리스트 BFS처럼 양방향을 중복 저장할 필요가 없다.
> - 메서드는 빈 그래프/정점 하나의 연결 비용을 0으로 정했다.
> - 결과를 쓸 때 `isPresent()`로 연결 성공을 확인한 뒤 `getAsLong()`을 읽는다.
> - 일반 Union-Find는 합친 집합을 다시 분리하거나 간선을 삭제하는 기능을 제공하지 않는다.

## 관련 개념

- [[그리디]] — 선택을 확정할 수 있는 이유
- [[DFS와 그래프]] — 탐색으로 연결 요소를 구하는 방법
- [[다익스트라]] — 시작점 기준의 최단거리와 구분
