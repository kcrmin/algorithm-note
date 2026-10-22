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
    if (n <= 1) return n;
    if (computed[n]) return memo[n]; // 같은 상태의 하위 호출을 다시 펼치지 않음
    memo[n] = (solve(n - 1) + solve(n - 2)) % MOD;
    computed[n] = true; // 값 0도 답이므로 계산 여부를 별도로 저장
    return memo[n];
}
```

Fibonacci, n≥0; `memo=new long[n+1]`, `computed=new boolean[n+1]`, `MOD=1_000_000_007`; 큰 n은 반복 DP. O(n).

**분류** [[00 Maps/Dynamic Programming|Dynamic Programming]]

**비교** [[40 Optimization/DP|DP]]
