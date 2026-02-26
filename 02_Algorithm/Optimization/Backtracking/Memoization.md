---
type: technique
tags:
  - memoization
  - dp
language: java
status: in-review
source: []
---

# Memoization (메모이제이션)

> **한 줄 요약:** 같은 상태에서의 재귀 결과를 저장해 두고 다시 쓰는 기법 (DFS + 캐시)

## Core Concept

- **When to use:**
  - 재귀/DFS로 탐색하는데 **같은 상태가 여러 번 나올 때**
  - 부분 문제가 겹치는 경우 (DP를 재귀 형태로 쓸 때)
  - "이 상태에서의 답"이 항상 동일할 때만 적용 가능
- **Key Point:**
  - **상태**를 한 묶음으로 표현 (예: (위치, 남은 값) → 정수/비트/키)
  - 한 번 계산한 상태는 캐시에 넣고, 재진입 시 캐시 값 반환
  - 되돌리기(unchoose)가 없는 "순수 함수" 형태일 때만 메모이제이션 가능 — 상태가 인자만으로 결정될 때

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Cache init | `memo = new T[범위]; Arrays.fill(memo, sentinel);` | 미계산 표시 |
| Check | `if (memo[state] != sentinel) return memo[state];` | 이미 계산된 상태 |
| Store | `return memo[state] = result;` | 계산 후 저장 |

## Patterns

### Pattern A: 1차원 상태 (재귀 DP)

```java
long[] memo = new long[n + 1];
Arrays.fill(memo, -1);

long f(int i) {
    if (i <= 0) return 0;   // base
    if (memo[i] != -1) return memo[i];
    long res = f(i - 1) + f(i - 2);  // 또는 문제에 맞는 전이
    return memo[i] = res;
}
```

### Pattern B: 2차원 상태 (격자/선택)

```java
Long[][] memo = new Long[n][m];  // null = 미계산

long f(int i, int j) {
    if (i < 0 || j < 0) return 0;
    if (memo[i][j] != null) return memo[i][j];
    long res = Math.max(f(i - 1, j), f(i, j - 1)) + a[i][j];
    return memo[i][j] = res;
}
```

### Pattern C: Map으로 상태 캐싱 (복잡한 상태)

- 상태가 (정수 여러 개, 비트마스크 등)로 묶일 때

```java
Map<Long, Long> memo = new HashMap<>();

long f(int pos, int mask) {
    long key = (long) pos << 20 | mask;
    if (memo.containsKey(key)) return memo.get(key);
    // ... 계산 ...
    memo.put(key, res);
    return res;
}
```

## Pitfalls & Tips

- **메모이제이션 가능 조건**: 같은 (인자) 상태면 답이 항상 같아야 함. "선택 순서"에 따라 결과가 달라지면 캐시하면 안 됨 (순수 백트래킹).
- sentinel: -1, Long.MIN_VALUE, null 등 "아직 안 채움"과 구분 가능한 값 사용
- 상태 공간이 너무 크면 메모리/시간 초과 — 상태를 줄이거나 반복 DP로 전환
- Map 사용 시 key 인코딩이 일대일이어야 함 (충돌 없게)
