---
type: template
aliases: ["Dynamic Programming", "DP_Basics"]
---

# DP

```java
// n≥0. 1·2칸씩 올라가는 방법 수를 MOD로 나눈 나머지.
long MOD = 1_000_000_007L;
long[] dp = new long[n + 1]; // dp[i] = 정확히 i칸에 도착하는 방법 수 mod MOD
dp[0] = 1; // 아무 이동도 안 하는 한 가지가 첫 이동의 출발점이다.
// 작은 칸부터 채워야 읽는 두 이전 상태가 완성되어 있다.
for (int i = 1; i <= n; i++) {
    dp[i] = dp[i - 1];
    if (i >= 2) dp[i] += dp[i - 2]; // 마지막 1칸/2칸은 겹치지 않으므로 더한다.
    dp[i] %= MOD;
}
long answer = dp[n];
```

**예시**

```text
마지막 이동       1칸          2칸
출발 상태       dp[3]=3      dp[2]=2
                    ↘       ↙
                     dp[4]=5
칸 i      0  1  2  3  4
방법 수   1  1  2  3  5
```

**주의** 이 식은 계단 예제의 식이다. 다른 문제는 상태·전이식을 다시 세운다. O(n) 시간·공간.

[[Recursion and Memoization]] · [[Knapsack DP]] · [[LIS]] · [[Greedy]]
