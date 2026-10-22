---
type: template
aliases: ["Dynamic Programming", "DP_Basics"]
category: dynamic-programming
categories: [dynamic-programming]
---

# DP

```text
한 번에 1칸 또는 2칸 → 4칸에 도착하는 방법 수

도착 칸     0   1   2   3   4
방법 수     1   1   2   3   5

[3칸까지 3가지] ── 마지막 1칸 ──┐
                               ├→ [4칸까지 3+2 = 5가지]
[2칸까지 2가지] ── 마지막 2칸 ──┘
```

| Step | Pseudocode |
| --- | --- |
| State | // 각 칸까지 오는 방법 수를 저장한다 |
| Seed | // 0칸에 머무는 방법 한 가지에서 시작한다 |
| Transition | // 마지막 이동이 1칸인 경우와 2칸인 경우를 더한다 |
| Answer | // 목표 칸에 저장된 방법 수를 반환한다 |

## Java

```java
long countWays(int n, long mod) {
    long[] dp = new long[n + 1];
    dp[0] = 1 % mod;

    for (int i = 1; i <= n; i++) {
        dp[i] = dp[i - 1];
        if (i >= 2) {
            dp[i] += dp[i - 2];
        }
        dp[i] %= mod;
    }

    return dp[n];
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 상태부터 정의 | 여기서는 `dp[i]`가 **i칸까지 오는 방법 수**. 다른 문제에 이 점화식을 그대로 쓰는 것은 아니다 |
| `dp[0] = 1 % mod` | 아직 이동하지 않은 경로가 1개. 나머지를 저장하므로 mod=1이면 저장값은 0 |
| 작은 칸 → 큰 칸 | 현재 값이 읽는 이전 두 칸이 먼저 계산되어 있어야 한다 |
| 더하기 / min / max | **방법 수**는 더하기. 최소 비용·최대 가치는 각각 그 목적에 맞는 비교와 초기값이 필요하다 |

`n ≥ 0`, `1 ≤ mod ≤ Integer.MAX_VALUE` · 답은 나머지 · 시간·공간 `O(n)`.

**분류** [[00 Maps/Dynamic Programming|Dynamic Programming]]
