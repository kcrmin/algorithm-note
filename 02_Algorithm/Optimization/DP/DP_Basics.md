---
type: concept
tags:
  - dp
  - dynamic-programming
language: java
status: in-review
source: []
---

# DP Basics (동적 계획법 기초)

> **한 줄 요약:** 부분 문제의 답을 저장해 재사용하며 전체 최적해를 구하는 방법

## Core Concept

- **When to use:** 최적 부분 구조 + 겹치는 부분 문제가 있을 때
- **Key Point:**
  - **상태 정의**: dp[i] 또는 dp[i][j]가 무엇을 의미하는지 명확히
  - **점화식**: 이전 상태에서 현재 상태로 오는 전이
  - **초기값·순서**: base case와 채우는 순서(의존 관계에 맞게)
  - 메모이제이션(재귀) 또는 테이블(반복)으로 구현

## API & State Change

| 관점 | 설명 |
| :--- | :--- |
| 상태 | dp[i], dp[i][j] 등으로 정의 |
| 전이 | dp[new] = f(dp[old], ...) |
| 초기값 | dp[0], dp[0][0] 등 |
| 순서 | 1D: 앞→뒤 또는 뒤→앞, 2D: 행/열 순서 |

## Patterns

### Pattern A: 1D DP (피보나치형)

```java
long[] dp = new long[n + 1];
dp[0] = 0;
dp[1] = 1;
for (int i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
}
```

### Pattern B: 메모이제이션 (재귀)

```java
long[] memo = new long[n + 1];
Arrays.fill(memo, -1);
long f(int i) {
    if (i <= 1) return i;
    if (memo[i] != -1) return memo[i];
    return memo[i] = f(i - 1) + f(i - 2);
}
```

### Pattern C: 2D DP (경로·선택)

```java
// dp[i][j]: (i,j)까지의 최적값
long[][] dp = new long[H][W];
dp[0][0] = a[0][0];
for (int i = 0; i < H; i++) {
    for (int j = 0; j < W; j++) {
        if (i == 0 && j == 0) continue;
        long from = Long.MAX_VALUE;
        if (i > 0) from = Math.min(from, dp[i - 1][j]);
        if (j > 0) from = Math.min(from, dp[i][j - 1]);
        dp[i][j] = from + a[i][j];
    }
}
```

## Pitfalls & Tips

- 초기값을 잘못 두면 전체가 틀림
- 순서: dp[i]가 dp[i-1] 등만 쓰면 앞에서 뒤로; 뒤를 먼저 써야 하면 뒤에서 앞으로
- 공간 최적화: 1D만 쓰면 이전 행만 유지하면 될 때가 많음
- 음수/무한 초기화: 최소화면 INF, 최대화면 -INF 등
