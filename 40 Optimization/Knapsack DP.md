---
type: template
aliases: ["0-1 Knapsack", "Knapsack"]
---

# Knapsack DP

```java
long[] dp = new long[capacity + 1];
for (int i = 0; i < weight.length; i++) {
    for (int w = capacity; w >= weight[i]; w--) {  // ①
        dp[w] = Math.max(dp[w],
                dp[w - weight[i]] + value[i]);  // ②
    }
}
long answer = dp[capacity];
```

| 암기 포인트 | 이유 |
|---|---|
| ① 용량을 큰 쪽부터 | 작은 용량 칸은 아직 이번 물건을 넣기 전 상태다. |
| ② 안 넣기 vs 넣기 | 기존 dp[w]와 남은 용량의 가치 + 현재 가치를 비교한다. |
| 무제한이면 정순 | 갱신된 작은 칸을 다시 읽어 같은 물건을 여러 번 쓴다. |

**예시**

```text
물건 하나: 무게 2, 가치 3 / 용량 4
역순 w:  4  3  2     → dp[4]=3  (한 번)
정순 w:  2  3  4     → dp[4]=6  (두 번!)
                 dp[2]=3을 다시 읽기 때문
```

**적용** 각 물건을 최대 한 번 고르며, 무게 합이 capacity 이하인 최대 가치를 구한다.

**준비** `dp[w]` = 용량 w 이하의 최대 가치. 안 골라도 되므로 처음은 0.

**주의** weight > 0. '정확히 w'를 채우는 문제는 불가능 상태를 별도로 둔다. O(n·capacity) 시간, O(capacity) 공간.

[[DP]] · [[LIS]]
