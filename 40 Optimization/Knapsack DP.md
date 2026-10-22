---
type: template
aliases: ["0-1 Knapsack", "Knapsack"]
---

# Knapsack DP

```java
// weight[i]>0. dp[w] = 용량 w 이하의 최대 가치, 아무것도 안 고른 초기 가치는 0.
long[] dp = new long[capacity + 1];
for (int i = 0; i < weight.length; i++) {
    // 역순이어야 이번 물건을 넣기 전의 작은 칸을 읽는다.
    for (int w = capacity; w >= weight[i]; w--) {
        dp[w] = Math.max(dp[w],
                // 안 고른 가치 vs 남은 용량의 가치+현재 물건.
                dp[w - weight[i]] + value[i]);
    }
}
long answer = dp[capacity];
```

**예시**

```text
물건 하나: 무게 2, 가치 3 / 용량 4
역순 w:  4  3  2     → dp[4]=3  (한 번)
정순 w:  2  3  4     → dp[4]=6  (두 번!)
                 dp[2]=3을 다시 읽기 때문
```

**주의** weight > 0. '정확히 w'를 채우는 문제는 불가능 상태를 별도로 둔다. O(n·capacity) 시간, O(capacity) 공간.

[[DP]] · [[LIS]]
