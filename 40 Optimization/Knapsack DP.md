---
type: template
aliases: ["0-1 Knapsack", "Knapsack"]
category: dynamic-programming
categories: [dynamic-programming]
---

# Knapsack DP

```text
물건: 무게 2, 가치 3       가방 용량: 4

용량        0   1   2   3   4
처리 전     0   0   0   0   0
0/1 결과    0   0   3   3   3   ← 한 개만 사용
무제한 결과 0   0   3   3   6   ← 두 개 사용 가능

0/1:   큰 용량 → 작은 용량   이전 상태를 읽는다
무제한: 작은 용량 → 큰 용량   이번 물건을 쓴 상태도 읽는다
```

| Step | Pseudocode |
| --- | --- |
| State | // 각 용량 이하로 담을 수 있는 최대 가치를 저장한다 |
| Pick | // 물건을 하나 정해 모든 가능한 용량에 반영한다 |
| Update | // 안 넣는 가치와, 남은 용량에 이번 물건을 넣는 가치를 비교한다 |
| Answer | // 가방 전체 용량에서의 최대 가치를 반환한다 |

## 0/1 Knapsack

각 물건을 **최대 한 번** 사용한다.

### Java

```java
long knapsack(int[] weight, int[] value, int capacity) {
    long[] dp = new long[capacity + 1];

    for (int i = 0; i < weight.length; i++) {
        // 역순: dp[w - weight[i]]는 이번 물건을 아직 쓰지 않은 값
        for (int w = capacity; w >= weight[i]; w--) {
            dp[w] = Math.max(dp[w], dp[w - weight[i]] + value[i]);
        }
    }

    return dp[capacity];
}
```

## Unbounded Knapsack

같은 물건을 **여러 번** 쓸 수 있으면 안쪽 용량 순서를 바꾼다.

| 선택 | 진행 | 위 예시에서 용량 4를 계산할 때 |
| --- | --- | --- |
| 0/1 | `capacity → weight[i]` | 용량 2의 **처리 전 0**을 읽어 결과 3 |
| Unbounded | `weight[i] → capacity` | 용량 2의 **이번에 갱신한 3**을 읽어 결과 6 |

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 용량 이하 / 정확한 무게 | 이 코드는 용량 **이하**라서 전부 0으로 시작. 정확히 채워야 하면 0번만 가능하고 나머지는 불가능 상태로 초기화해야 한다 |
| 불가능 상태 갱신 | 음의 무한대에 가치를 무조건 더하지 않는다. 이전 상태가 가능한지 먼저 확인 |
| 물건 순서 / 용량 순서 | 이 템플릿은 물건이 바깥 반복. 최대 가치가 아닌 경우의 수 문제는 반복 순서에 따라 중복 계산의 의미도 달라진다 |

`weight[i] > 0`, 두 배열 길이 동일, `capacity ≥ 0` · 시간 `O(n × capacity)`, 공간 `O(capacity)`.

**분류** [[00 Maps/Dynamic Programming|Dynamic Programming]]

**사용 원리** [[40 Optimization/DP|DP]]
