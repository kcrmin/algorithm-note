---
type: template
tags: ["Search"]
aliases: ["Memoization", "Top-down DP"]
category: dynamic-programming
categories: [dynamic-programming]
---

# Recursion and Memoization

```java
static long[] memo;
static boolean[] computed;
static final long MOD = 1_000_000_007;

static long solve(int n) {
    // base: 더 쪼갤 수 없는 상태 반환
    if (n <= 1) return n;
    // reuse: 계산한 상태는 저장값 반환
    if (computed[n]) return memo[n];
    // compute: 작은 상태의 답을 합쳐 저장
    memo[n] = (solve(n - 1) + solve(n - 2)) % MOD;
    computed[n] = true;
    return memo[n];
}
```

Fibonacci, n≥0; `memo=new long[n+1]`, `computed=new boolean[n+1]`, `MOD=1_000_000_007`; 큰 n은 반복 DP. O(n).

**분류** [[00 Maps/Dynamic Programming|Dynamic Programming]]

**비교** [[40 Optimization/DP|DP]]
