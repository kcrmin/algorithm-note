---
type: template
tags: ["DataStructures"]
aliases: ["Sparse_Table"]
category: range-queries
categories: [range-queries]
---

# Sparse Table

```java
int[][] table;
void build(int[] arr) {
    int n = arr.length;
    int levels = 32 - Integer.numberOfLeadingZeros(n);
    table = new int[levels][n];
    table[0] = arr.clone();
    for (int k = 1; k < levels; k++) {
        int half = 1 << (k - 1);
        for (int i = 0; i + (1 << k) <= n; i++) {
            table[k][i] = Math.min(table[k - 1][i], table[k - 1][i + half]);
        }
    }
}
int min(int left, int right) {
    int k = 31 - Integer.numberOfLeadingZeros(right - left + 1);
    // 양끝의 2^k 구간이 겹쳐도 min은 같아 O(1)에 덮는다.
    return Math.min(table[k][left], table[k][right - (1 << k) + 1]);
}
```

고정된 비어 있지 않은 배열, 0-index [left,right] · min/max/GCD처럼 중복 무관한 연산용 · 전처리·공간 O(n log n), 질의 O(1).

**분류** [[00 Maps/Range Queries|Range Queries]]

**비교** [[60 Range Data Structures/Segment Tree|Segment Tree]]
