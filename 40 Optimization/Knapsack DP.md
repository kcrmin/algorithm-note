---
type: template
aliases: ["0-1 Knapsack", "Knapsack"]
category: dynamic-programming
categories: [dynamic-programming]
---

# Knapsack DP

```java
long knapsack(int[] weight, int[] value, int capacity) {
    // state: dp[w]=용량 w 이하의 최대 가치
    long[] dp = new long[capacity + 1];
    // pick: 이번 물건을 용량별로 반영
    for (int i = 0; i < weight.length; i++) {
        // update: 역순으로 이전 물건 상태만 읽어 중복 방지
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
