---
type: template
tags: [그래프]
aliases: [Lowest Common Ancestor, 최소 공통 조상, 이진 리프팅]
---

# LCA

> 두 정점의 깊이를 맞춘 뒤, 조상이 서로 다른 동안 함께 올라가면 마지막에 같은 부모를 만난다.

## 개념

LCA(최소 공통 조상)는 루트가 정해진 트리에서 두 정점의 공통 조상 중 깊이가 가장 깊은 정점이다. 각 정점의 `2^k`번째 조상을 미리 저장하면 긴 부모 경로를 이진수의 자리처럼 나누어 건너뛰어 조상과 경로 질문에 빠르게 답할 수 있다.

## 적용 조건

- 정점 `0 ~ n-1`, `n >= 1`인 연결된 무방향 트리와 유효한 root를 받는다. 자기 자신도 자신의 조상에 포함하므로 `lca(v,v) = v`다.
- `edges[i] = {정점1, 정점2}`다. 간선은 정확히 `n-1`개여야 하며 BFS로 모든 정점에 도달해야 한다. 입력의 정점 번호는 유효하다고 가정한다.
- `up[k][v]`는 v에서 부모 방향으로 `2^k`번 이동한 정점이다. 루트 위로 넘어가는 표의 값은 루트로 유지한다.
- 전처리 시간·공간 `O(n log n)`, LCA·k번째 조상·두 정점 거리 질의는 각각 `O(log n)`이다.
- `kthAncestor(v,k)`는 k가 v의 깊이보다 크면 `-1`을 반환한다. `distance(a,b)`는 경로의 **간선 수**를 `long`으로 반환한다.
- 부모와 깊이는 반복 BFS로 구하므로 일자형 트리에서도 전처리에 재귀 호출을 사용하지 않는다.

## Java 템플릿

```java
import java.util.*;

class BinaryLiftingLca {
    final int[][] up;
    final int[] depth;
    final int log;

    // n >= 1, 정점 0..n-1, 연결된 무방향 트리, 유효한 root.
    BinaryLiftingLca(int n, int[][] edges, int root) {
        if (n <= 0 || root < 0 || root >= n || edges.length != n - 1) {
            throw new IllegalArgumentException("invalid tree or root");
        }
        int levels = 1;
        while ((1L << levels) <= n) levels++;
        log = levels;
        up = new int[log][n];
        depth = new int[n];
        Arrays.fill(depth, -1);

        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }

        Queue<Integer> queue = new ArrayDeque<>();
        depth[root] = 0;
        up[0][root] = root;
        queue.offer(root);
        int visitedCount = 0;
        while (!queue.isEmpty()) {
            int current = queue.poll();
            visitedCount++;
            for (int next : graph.get(current)) {
                if (depth[next] != -1) continue;
                depth[next] = depth[current] + 1;
                up[0][next] = current;
                queue.offer(next);
            }
        }
        if (visitedCount != n) throw new IllegalArgumentException("disconnected tree");

        for (int k = 1; k < log; k++) {
            for (int vertex = 0; vertex < n; vertex++) {
                up[k][vertex] = up[k - 1][up[k - 1][vertex]];
            }
        }
    }

    int lca(int a, int b) {
        if (depth[a] < depth[b]) {
            int temporary = a;
            a = b;
            b = temporary;
        }
        a = kthAncestor(a, depth[a] - depth[b]);
        if (a == b) return a;

        for (int k = log - 1; k >= 0; k--) {
            if (up[k][a] != up[k][b]) {
                a = up[k][a];
                b = up[k][b];
            }
        }
        return up[0][a];
    }

    int kthAncestor(int vertex, int steps) {
        if (steps < 0) throw new IllegalArgumentException("steps must be nonnegative");
        if (steps > depth[vertex]) return -1;
        for (int k = 0; k < log; k++) {
            if (((steps >>> k) & 1) != 0) vertex = up[k][vertex];
        }
        return vertex;
    }

    long distance(int a, int b) {
        int common = lca(a, b);
        return (long) depth[a] + depth[b] - 2L * depth[common];
    }
}
```

> [!info]- 원리와 코드 설명
>
> ### 1. 부모를 두 배씩 건너뛰는 표
>
> 루트 0에 간선 `0-1, 0-2, 1-3, 1-4, 2-5, 5-6`이 있는 트리다.
>
> ```text
> 0
> ├─ 1
> │  ├─ 3
> │  └─ 4
> └─ 2
>    └─ 5
>       └─ 6
> ```
>
> | v | 깊이 | `up[0][v]`: 1칸 위 | `up[1][v]`: 2칸 위 |
> |---:|---:|---:|---:|
> | 0 | 0 | 0 | 0 |
> | 1 | 1 | 0 | 0 |
> | 2 | 1 | 0 | 0 |
> | 3 | 2 | 1 | 0 |
> | 4 | 2 | 1 | 0 |
> | 5 | 2 | 2 | 0 |
> | 6 | 3 | 5 | 2 |
>
> 6에서 두 칸 위는 `6→5→2`이므로 `up[1][6] = 2`다. 네 칸 위는 두 칸 위로 두 번 이동한 `up[1][up[1][6]] = up[1][2] = 0`이다. 루트의 부모를 자기 자신으로 두었기 때문에 루트 위를 조회하더라도 배열 인덱스가 `-1`이 되지 않는다.
>
> 일반식은 `up[k][v] = up[k-1][up[k-1][v]]`다. 위 표의 행이 완성되어 있어야 다음 행을 만들 수 있으므로 k를 바깥 반복으로 둔다.
>
> ### 2. 깊이가 다른 두 정점을 맞추기
>
> `lca(6,4)`에서 깊이는 각각 3과 2다. 깊이 차이 1만큼 6을 올리면 `(5,4)`가 되어 깊이가 모두 2다. 깊이를 맞추는 동안 같은 정점이 되면 그 정점이 LCA다. 예를 들어 `lca(6,2)`는 6을 두 칸 올리는 것만으로 2가 된다.
>
> `kthAncestor`는 이동할 칸 수를 이진수로 나눈다. 13칸이면 `13 = 8+4+1`이므로 1칸, 4칸, 8칸 점프를 조합한다. 모든 정점의 부모를 한 칸씩 따라가는 대신 `O(log n)`번만 표를 조회한다.
>
> ### 3. 조상이 다를 때만 두 정점을 함께 올리는 이유
>
> 깊이를 맞춘 `(5,4)`에 대해 큰 점프부터 비교한다.
>
> | 점프 크기 | 5의 점프 결과 | 4의 점프 결과 | 처리 |
> |---|---:|---:|---|
> | 4칸 | 0 | 0 | 같으므로 유지 |
> | 2칸 | 0 | 0 | 같으므로 유지 |
> | 1칸 | 2 | 1 | 다르므로 `(2,1)`로 이동 |
>
> 두 점프 결과가 다르면 둘 다 아직 LCA 아래에 있다. 결과가 같을 때까지 무작정 올리면 LCA를 지나 더 위의 공통 조상으로 갈 수 있으므로, **다를 때만** 이동한다.
>
> 반복이 끝났을 때 `(2,1)`은 서로 다르지만 바로 위 부모는 0으로 같다. 따라서 `up[0][2] = 0`이 답이다. 같은 깊이의 `lca(3,4)`는 모든 점프에서 조상이 같아 두 정점을 유지하고 부모 1을 반환한다.
>
> ### 4. 경로 길이는 공통으로 겹치는 루트 구간을 뺀다
>
> 루트에서 a까지의 간선 수가 `depth[a]`, 루트에서 b까지의 간선 수가 `depth[b]`다. 두 경로는 루트부터 LCA까지의 구간이 겹친다.
>
> 따라서 `distance(a,b) = depth[a] + depth[b] - 2×depth[lca(a,b)]`다. 6과 4는 `3+2-2×0 = 5`이고, 실제 경로는 `6→5→2→0→1→4`로 간선이 5개다. 3과 4는 `2+2-2×1 = 2`다.
>
> 코드는 덧셈 전부터 한 항을 long으로 바꾼다. 최종 거리가 int 범위 안에 있어도 중간 덧셈은 더 커질 수 있기 때문이다.
>
> ### 경계 조건과 변형
>
> - n=1이면 루트의 깊이는 0, 자기 자신과의 LCA는 루트, 거리는 0이다.
> - 표에서는 루트를 계속 부모로 사용하지만 `kthAncestor`는 먼저 깊이를 검사한다. 따라서 루트보다 위의 조상을 요청했을 때 루트가 답인 것처럼 반환하지 않는다.
> - 루트를 바꾸면 부모·깊이·LCA는 바뀐다. 무향 트리에서 두 정점 사이의 경로와 거리는 루트 선택과 무관하다.
> - 간선 가중치가 있으면 루트부터의 누적 거리 `rootDistance[v]`를 별도로 저장하고 `rootDistance[a] + rootDistance[b] - 2×rootDistance[lca(a,b)]`로 가중치 합을 구한다. 위 코드는 가중치 없는 간선 수다.
> - 경로의 최솟값·최댓값을 질의하려면 `up[k][v]`와 함께 같은 점프 구간의 집계 값을 저장할 수 있다.
> - a에서 b로 가는 경로의 0번째 정점을 a로 정의하자. LCA까지의 거리가 x, LCA에서 b까지가 y이면, k번째 정점은 `k <= x`에서 a의 k번째 조상이고, 그 뒤는 b의 `(x+y-k)`번째 조상이다. 유효한 k는 `0 ~ x+y`다.
> - 연결되지 않은 숲이나 일반 그래프는 위 생성자의 대상이 아니다. 조상 관계를 정의할 루트 트리가 필요하다.
>
> ## 연결되는 개념
>
> - [[트리와 재귀]] — 루트, 부모, 깊이와 유일한 경로
> - [[BFS]] — 재귀 없이 부모와 깊이 배열 만들기
> - [[유니온 파인드와 MST]] — MST로 얻은 트리에서 경로의 최댓값을 질의하는 조합
