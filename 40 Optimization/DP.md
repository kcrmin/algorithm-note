---
type: template
aliases: ["Dynamic Programming", "DP_Basics"]
category: dynamic-programming
categories: [dynamic-programming]
---

# DP

```java
long countWays(int n, long mod) {
    long[] dp = new long[n + 1];
    // seed: 아무 이동도 안 한 한 가지에서 시작
    dp[0] = 1 % mod;
    // transition: 마지막 1칸·2칸의 이전 상태를 합산
    for (int i = 1; i <= n; i++) {
        dp[i] = dp[i - 1];
        if (i >= 2) dp[i] += dp[i - 2];
        dp[i] %= mod;
    }
    // answer: 목표 칸에 도착하는 방법 수
    return dp[n];
}
```

1·2칸 계단 방법 수의 점화식 예제 · n≥0, 1≤mod≤Integer.MAX_VALUE · 시간·공간 O(n).

**분류** [[00 Maps/Dynamic Programming|Dynamic Programming]]
