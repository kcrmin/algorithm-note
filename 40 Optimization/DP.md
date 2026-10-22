---
type: template
aliases: ["Dynamic Programming", "DP_Basics"]
---

# DP

**언제** 같은 상태가 반복되면 답을 저장한다. 아래는 1·2칸씩 올라 정확히 n칸에 도착하는 방법 수를 MOD로 나눈 나머지다.

```text
마지막 이동       1칸          2칸
출발 상태       dp[3]=3      dp[2]=2
                    ↘       ↙
                     dp[4]=5
칸 i      0  1  2  3  4
방법 수   1  1  2  3  5
```

**코드** `dp[i]` = 정확히 i칸에 도착하는 방법 수 mod MOD.

```java
long MOD = 1_000_000_007L;
long[] dp = new long[n + 1];
dp[0] = 1;                         // ①
for (int i = 1; i <= n; i++) {
    dp[i] = dp[i - 1];
    if (i >= 2) dp[i] += dp[i - 2]; // ②
    dp[i] %= MOD;
}
long answer = dp[n];
```

| 암기 포인트 | 이유 |
|---|---|
| 상태 → 식 → 초기값 → 순서 | 무엇을 세는 칸인지 정해야 식과 순서가 결정된다. |
| ① `dp[0]=1` | 아무 이동도 안 하는 방법 하나가 첫 이동의 출발점이다. |
| ② 이전 두 칸을 더함 | 마지막 이동이 1칸인 경우와 2칸인 경우는 겹치지 않는다. |

**주의** 이 식은 계단 예제의 식이다. 다른 문제는 상태·전이식을 다시 세운다. O(n) 시간·공간.

[[Recursion and Memoization]] · [[Knapsack DP]] · [[LIS]] · [[Greedy]]
