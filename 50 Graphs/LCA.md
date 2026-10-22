---
type: template
tags: ["Graphs"]
aliases: ["Lowest Common Ancestor", "Binary Lifting"]
category: traversal-trees
categories: [traversal-trees]
---

# LCA

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
            up[k][v] = up[k - 1][up[k - 1][v]]; // 절반 거리의 조상을 두 번 적용
        }
    }
}
static int lca(int a, int b) {
    if (depth[a] < depth[b]) { int t = a; a = b; b = t; }
    int difference = depth[a] - depth[b]; // 같은 깊이로 맞춰야 공통 조상을 비교 가능
    for (int k = 0; k < log; k++) {
        if (((difference >>> k) & 1) != 0) a = up[k][a];
    }
    if (a == b) return a;
    for (int k = log - 1; k >= 0; k--) {
        if (up[k][a] != up[k][b]) { // 조상이 같을 때 뛰면 LCA를 지나치므로 다를 때만
            a = up[k][a];
            b = up[k][b];
        }
    }
    return up[0][a];
}
```

[[Tree and Parent Array]]의 `parent`·`depth`, 루트 부모=자기 자신; `buildTable(n)`→`lca(a,b)`. 전처리 O(V log V), 질의 O(log V).

**분류** [[00 Maps/Traversal and Trees|Traversal and Trees]]

**필요 코드** [[50 Graphs/Tree and Parent Array|Tree and Parent Array]]
