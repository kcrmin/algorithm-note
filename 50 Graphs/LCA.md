---
type: template
tags: ["Graphs"]
aliases: ["Lowest Common Ancestor", "Binary Lifting"]
---

# LCA

**언제** 루트 Tree에서 두 정점의 가장 가까운 공통 조상을 여러 번 구한다.

```text
부모 관계: 0 → {1,2}, 1 → 3, 2 → 5 → 6
lca(6,3): 깊이 맞춤 (6,3) → (5,3)
          부모가 다름 → (2,1)
          이제 부모가 같음 → 답 0
```

**코드** `Tree and Parent Array`로 parent/depth 준비 → `buildTable(n)` 호출. 필드 `int log; int[][] up;`. `up[k][v]`=2ᵏ번째 조상.

```java
static void buildTable(int n) {
    log = 32 - Integer.numberOfLeadingZeros(n);
    up = new int[log][n];
    up[0] = parent;
    for (int k = 1; k < log; k++) {
        for (int v = 0; v < n; v++) {
            up[k][v] = up[k - 1][up[k - 1][v]]; // ①
        }
    }
}
static int lca(int a, int b) {
    if (depth[a] < depth[b]) { int t = a; a = b; b = t; }
    int difference = depth[a] - depth[b];
    for (int k = 0; k < log; k++) {
        if (((difference >>> k) & 1) != 0) a = up[k][a];
    }
    if (a == b) return a;
    for (int k = log - 1; k >= 0; k--) {
        if (up[k][a] != up[k][b]) { // ②
            a = up[k][a];
            b = up[k][b];
        }
    }
    return up[0][a];
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 2ᵏ = 2ᵏ⁻¹ + 2ᵏ⁻¹ | 절반 거리 조상을 두 번 따라가 표를 만든다. |
| 깊이부터 같게 | 같은 높이여야 두 점프의 조상을 비교할 수 있다. |
| ② 조상이 다를 때만 점프 | 같을 때 뛰면 답을 지나친다. 답 바로 아래까지 올라간다. |

**주의** 루트 부모는 자기 자신. 전처리 O(V log V), 질의 O(log V), 공간 O(V log V).

[[Tree and Parent Array]] · [[Sparse Table]]
