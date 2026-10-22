---
type: template
tags: ["Graphs"]
aliases: ["Lowest Common Ancestor", "Binary Lifting"]
category: traversal-trees
categories: [traversal-trees]
---

# LCA

```text
          0
         / \
        1   2
       / \
      3   4
      |
      5

LCA(5,4): 5를 3까지 올림 → 3과 4의 부모 1
점프 표: 1칸 · 2칸 · 4칸 · 8칸 … 위의 조상
```

| Step | Pseudocode |
|---|---|
| Build | // 두 번의 절반 점프로 두 배 거리의 조상을 만든다. |
| Align | // 더 깊은 정점을 깊이 차이만큼 올려 높이를 맞춘다. |
| Lift | // 큰 점프부터 보며 도착 조상이 서로 다를 때만 둘을 함께 올린다. |
| Answer | // 둘이 같아졌으면 그 정점, 아니면 바로 위 부모가 공통 조상이다. |

## Java

```java
static int log;
static int[][] up;
static int[] parent, depth;

static void buildTable(int n) {
    log = 32 - Integer.numberOfLeadingZeros(n);
    up = new int[log][n];
    up[0] = parent;

    for (int k = 1; k < log; k++) {
        for (int v = 0; v < n; v++) {
            up[k][v] = up[k - 1][up[k - 1][v]];
        }
    }
}

static int lca(int a, int b) {
    if (depth[a] < depth[b]) {
        int t = a;
        a = b;
        b = t;
    }

    int difference = depth[a] - depth[b];

    for (int k = 0; k < log; k++) {
        if (((difference >>> k) & 1) != 0) {
            a = up[k][a];
        }
    }

    if (a == b) {
        return a;
    }

    for (int k = log - 1; k >= 0; k--) {
        // 공통 조상으로 합쳐지기 직전까지만 올린다.
        if (up[k][a] != up[k][b]) {
            a = up[k][a];
            b = up[k][b];
        }
    }

    return up[0][a];
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| `up[k][v]` | v의 2^k번째 조상. 2^(k-1)번째 조상에서 같은 거리를 한 번 더 간다. |
| 깊이 정렬 후 같은 정점 | 한쪽이 다른 쪽의 조상이면 여기서 바로 답이다. |
| 조상이 다를 때만 상승 | 같은 조상으로 올리면 정답을 지나칠 수 있다. 공통 조상 바로 아래에서 멈춘다. |
| 큰 k → 작은 k | 공통 조상 아래에서 가능한 가장 큰 점프를 먼저 쓴다. |
| 루트 부모=자기 자신 | 높이보다 큰 점프도 유효 인덱스에 머문다. parent·depth를 먼저 준비한다. |

`buildTable(n)` 후 `lca(a,b)`. n≥1. parent·depth는 아래 연결된 Tree and Parent Array 사용. 전처리 **O(V log V)**, 질의 **O(log V)**.

**분류** [[00 Maps/Traversal and Trees|Traversal and Trees]]

**필요 코드** [[50 Graphs/Tree and Parent Array|Tree and Parent Array]]
