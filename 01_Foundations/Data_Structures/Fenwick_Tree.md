---
type: data-structure
tags:
  - fenwick-tree
  - bit
language: java
status: in-review
source: []
---

# Fenwick Tree (Binary Indexed Tree, BIT)

> **한 줄 요약:** 구간 합을 O(log N)에 갱신·조회하는 트리 (1-index 기반)

## Core Concept

- **When to use:** 점 갱신 + 구간 합 쿼리가 반복될 때, 좌표 압축과 함께 역순 개수 등
- **Key Point:**
  - 인덱스 i가 관리하는 구간: i에서 맨 아래 1을 뺀 비트만큼 왼쪽으로 (i - (i & -i) + 1) ~ i
  - 합: i에서 0이 될 때까지 `i -= (i & -i)` 하며 누적
  - 갱신: i에서 n 이하까지 `i += (i & -i)` 하며 갱신
  - 1-index 사용 (0이면 무한 루프)

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Init | `long[] tree = new long[n+1];` | 1..n, 0은 비움 |
| Add | `add(i, delta)` | i번 위치에 delta 더함 |
| Prefix sum | `sum(i)` | [1, i] 구간 합 |
| Range sum | `sum(r) - sum(l-1)` | [l, r] 구간 합 |

## Patterns

### Pattern A: 구간 합 Fenwick Tree

```java
class Fenwick {
    int n;
    long[] tree;

    Fenwick(int n) {
        this.n = n;
        this.tree = new long[n + 1];
    }

    void add(int i, long delta) {
        for (; i <= n; i += (i & -i)) tree[i] += delta;
    }

    long sum(int i) {
        long s = 0;
        for (; i > 0; i -= (i & -i)) s += tree[i];
        return s;
    }

    long rangeSum(int l, int r) {
        return sum(r) - sum(l - 1);
    }
}
```

### Pattern B: 역순 개수 (좌표 압축 후)

```java
Fenwick fw = new Fenwick(M);  // M = 서로 다른 값 개수
long inv = 0;
for (int i = 0; i < n; i++) {
    int c = comp[i];  // 0..M-1
    inv += fw.rangeSum(c + 1, M);  // c보다 큰 것 개수
    fw.add(c + 1, 1);  // 1-index
}
```

## Pitfalls & Tips

- 인덱스는 1부터 (0 사용 시 루프)
- 구간 [l, r]은 sum(r) - sum(l-1)
- long overflow: 합이 크면 long 사용
- 좌표 압축과 함께 쓰일 때 압축 인덱스 0..M-1이면 Fenwick 크기는 M+1 (1-index)
