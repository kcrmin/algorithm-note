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
    dp[0] = 1 % mod; // 아무 이동도 안 한 한 가지가 첫 이동의 출발점이다.
    for (int i = 1; i <= n; i++) {
        dp[i] = dp[i - 1];
        if (i >= 2) dp[i] += dp[i - 2]; // 마지막 1칸·2칸은 겹치지 않는다.
        dp[i] %= mod;
    }
    return dp[n];
}
```

1·2칸 계단 방법 수의 점화식 예제 · n≥0, 1≤mod≤Integer.MAX_VALUE · 시간·공간 O(n).

**분류** [[00 Maps/Dynamic Programming|Dynamic Programming]]
