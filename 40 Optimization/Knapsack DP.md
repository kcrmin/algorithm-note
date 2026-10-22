---
type: template
aliases: ["0-1 Knapsack", "Knapsack"]
category: dynamic-programming
categories: [dynamic-programming]
---

# Knapsack DP

```java
long knapsack(int[] weight, int[] value, int capacity) {
    long[] dp = new long[capacity + 1]; // dp[w]: 용량 w 이하의 최대 가치.
    for (int i = 0; i < weight.length; i++) {
        // 역순이어야 이번 물건을 넣기 전 상태를 읽어 중복 선택을 막는다.
        for (int w = capacity; w >= weight[i]; w--) {
            dp[w] = Math.max(dp[w], dp[w - weight[i]] + value[i]);
        }
    }
    return dp[capacity];
}
```

weight[i]>0, 각 물건 1개 · 정확한 무게를 채우려면 불가능 상태를 별도로 둔다 · 시간 O(n·capacity), 공간 O(capacity).

**분류** [[00 Maps/Dynamic Programming|Dynamic Programming]]

**사용 원리** [[40 Optimization/DP|DP]]
