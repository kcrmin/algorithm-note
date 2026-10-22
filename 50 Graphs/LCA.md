---
type: template
tags: ["Graphs"]
aliases: ["Lowest Common Ancestor", "Binary Lifting"]
---

# LCA

```java
// Tree and Parent Array 카드로 parent/depth 준비; 루트 부모는 자기 자신.
// up[k][v]=v의 2^k번째 조상. buildTable(n) 후 lca(a,b) 호출.
static void buildTable(int n) {
    log = 32 - Integer.numberOfLeadingZeros(n);
    up = new int[log][n];
    up[0] = parent;
    for (int k = 1; k < log; k++) {
        for (int v = 0; v < n; v++) {
            // 2^k칸은 2^(k-1)칸을 두 번 올라간 거리
            up[k][v] = up[k - 1][up[k - 1][v]];
        }
    }
}
static int lca(int a, int b) {
    if (depth[a] < depth[b]) { int t = a; a = b; b = t; }
    // 먼저 같은 높이로 맞춰야 두 조상을 비교할 수 있음
    int difference = depth[a] - depth[b];
    for (int k = 0; k < log; k++) {
        if (((difference >>> k) & 1) != 0) a = up[k][a];
    }
    if (a == b) return a; // 한 정점이 다른 정점의 조상이면 이미 답에 도착
    for (int k = log - 1; k >= 0; k--) {
        if (up[k][a] != up[k][b]) { // 같을 때 뛰면 답을 지나침: 다를 때만 함께 상승
            a = up[k][a];
            b = up[k][b];
        }
    }
    return up[0][a]; // 두 점은 답 바로 아래의 서로 다른 자식에 남음
}
```

**예시**

```text
부모 관계: 0 → {1,2}, 1 → 3, 2 → 5 → 6
lca(6,3): 깊이 맞춤 (6,3) → (5,3)
          부모가 다름 → (2,1)
          이제 부모가 같음 → 답 0
```

**주의** 루트 부모는 자기 자신. 전처리 O(V log V), 질의 O(log V), 공간 O(V log V).

[[Tree and Parent Array]] · [[Sparse Table]]
