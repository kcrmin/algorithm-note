---
type: template
tags: ["Search"]
aliases: ["Memoization", "Top-down DP"]
category: dynamic-programming
categories: [dynamic-programming]
---

# Recursion and Memoization

```text
F(4)
├─ F(3)
│  ├─ F(2) → 1 저장
│  └─ F(1) → 1
└─ F(2) → 저장한 1 재사용

같은 상태를 다시 계산하지 않음 → F(4)=3
```

| Step | Pseudocode |
|---|---|
| Base | // 더 작게 나눌 필요가 없는 상태의 답을 반환한다. |
| Reuse | // 이미 계산한 상태라면 저장한 답을 바로 반환한다. |
| Compute | // 작은 상태의 답을 합쳐 현재 답을 만든다. |
| Save | // 답과 계산 완료 표시를 저장하고 반환한다. |

## Java

```java
static long[] memo;
static boolean[] computed;
static final long MOD = 1_000_000_007;

static long solve(int n) {
    if (n <= 1) {
        return n;
    }

    if (computed[n]) {
        return memo[n];
    }

    memo[n] = (solve(n - 1) + solve(n - 2)) % MOD;
    computed[n] = true;
    return memo[n];
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| `computed`와 `memo` | 답이 0인 상태도 있다. 값이 0이라는 이유만으로 미계산이라고 판단하지 않는다. |
| 더 작은 상태로 이동 | 재귀 의존 관계에 순환이 있으면 결과를 저장하기 전에 다시 진입한다. |
| 상태를 빠짐없이 키에 포함 | 답이 위치와 남은 자원에 의존하면 memo도 두 정보를 모두 구분해야 한다. |
| 큰 n | 시간이 선형이어도 호출 깊이는 n이다. 큰 n에는 반복 DP를 쓴다. |

Fibonacci mod `1_000_000_007`, n≥0. `memo=new long[n+1]`, `computed=new boolean[n+1]`. **O(n)** 시간·공간.

**분류** [[00 Maps/Dynamic Programming|Dynamic Programming]]

**비교** [[40 Optimization/DP|DP]]
