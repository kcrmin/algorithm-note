---
type: algorithm
tags:
  - knapsack
  - dp
language: java
status: in-review
source: []
---

# Knapsack (배낭 문제)

> **한 줄 요약:** 무게 제한 안에서 물건을 골라 가치 합을 최대화하는 DP

## Core Concept

- **When to use:** 무게/비용 제한과 가치가 주어질 때, 부분집합 선택
- **Key Point:**
  - **0-1 배낭**: 각 물건 최대 1번 — dp[무게] = 최대 가치, 역순으로 갱신
  - **무한 배낭**: 각 물건 무한 개 — dp[무게] 정순 갱신
  - 상태: dp[w] = 무게 w까지 썼을 때 최대 가치

## API & State Change

| 문제 | 상태 | 전이 |
| :--- | :--- | :--- |
| 0-1 | dp[w] | w 역순: dp[w] = max(dp[w], dp[w-wi]+vi) |
| 무한 | dp[w] | w 정순: dp[w] = max(dp[w], dp[w-wi]+vi) |

## Patterns

### Pattern A: 0-1 Knapsack (1차원, 역순)

```java
int[] dp = new int[W + 1];
// dp[w] = 무게 w 이하로 얻을 수 있는 최대 가치
for (int i = 0; i < n; i++) {
    int w = weight[i], v = value[i];
    for (int j = W; j >= w; j--) {
        dp[j] = Math.max(dp[j], dp[j - w] + v);
    }
}
// 답: dp[W]
```

### Pattern B: 무한 배낭 (정순)

```java
int[] dp = new int[W + 1];
for (int j = w; j <= W; j++) {
    dp[j] = Math.max(dp[j], dp[j - w] + v);
}
```

### Pattern C: 2D (물건 인덱스 + 무게)

```java
// dp[i][w] = 0..i번 물건만 쓸 때 무게 w 이하 최대 가치
int[][] dp = new int[n + 1][W + 1];
for (int i = 1; i <= n; i++) {
    for (int j = 0; j <= W; j++) {
        dp[i][j] = dp[i - 1][j];
        if (j >= weight[i - 1])
            dp[i][j] = Math.max(dp[i][j], dp[i - 1][j - weight[i - 1]] + value[i - 1]);
    }
}
```

## Pitfalls & Tips

- 0-1에서 정순으로 돌리면 같은 물건을 여러 번 쓴 효과가 됨 → 역순
- W가 크면 “가치 기준” DP (dp[v] = 최소 무게)로 바꾸는 경우 있음
- 초기값: 0으로 두고 max로 갱신
