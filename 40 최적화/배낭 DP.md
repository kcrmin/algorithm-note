---
type: template
tags: [심화]
aliases: [Knapsack]
---

# 배낭 DP

0/1 배낭은 각 물건을 한 번만 골라 용량 이내의 최대 가치를 만든다.

> **기억:** 물건 하나씩 → 용량은 큰 쪽부터 → 넣는 경우와 안 넣는 경우 비교.

**전제:** weight[i]>0, capacity≥0. dp[w]=용량 w 이하의 최대 가치. O(n×capacity).

## 템플릿

```java
long[] dp = new long[capacity + 1];
for (int i = 0; i < weight.length; i++) {
    for (int w = capacity; w >= weight[i]; w--) {
        dp[w] = Math.max(dp[w], dp[w - weight[i]] + value[i]);
    }
}
```

## 왜 이렇게 쓰나

- 기존 dp[w]는 안 고른 경우, `dp[w-weight[i]]+value[i]`는 현재 물건을 고른 경우다.
- 뒤에서 갱신해야 `dp[w-weight[i]]`를 **이번 물건을 넣기 전 상태**로 읽는다.
- 앞에서 갱신하면 방금 넣은 물건을 다시 읽어 여러 번 쓰게 된다.

무게 2·가치 3인 물건 하나, 용량 4: 역순은 dp[4]=3. 정순이면 dp[2]=3을 다시 읽어 dp[4]=6이 되어 같은 물건을 두 번 쓴다.

> [!info]- 더 자세한 설명
>
> `dp[w]`는 지금까지 본 물건으로 **용량 w 이하**에서 만들 수 있는 최대 가치다. 정확히 w를 채운다는 뜻이 아니다. 아무것도 고르지 않을 수 있으므로 처음은 모두 0이다.
>
> 물건 하나가 무게 2·가치 3, 용량이 4라고 하자. 뒤에서 갱신하면 w=4에서 읽는 `dp[2]`는 아직 0이다. 따라서 `dp[4]=3`. 다음 w=3,2도 3이 된다. 한 물건이 한 번만 들어간다.
>
> 반대로 앞에서 갱신하면 w=2에서 `dp[2]=3`이 된 뒤 w=4가 그 값을 읽는다. `dp[4]=dp[2]+3=6`으로 같은 물건을 두 번 쓰게 된다. **이전 물건까지의 상태를 읽기 위해 역순**인 것이다. 물건을 무제한 재사용하는 문제에서는 이 앞 방향이 의미가 있다.
>
> ### 주의점
>
> - weight와 value는 길이가 같아야 한다.
> - 정확한 무게를 채우는 문제라면 도달 불가 상태를 0으로 초기화하면 안 된다.
> - 경우의 수를 세는 문제는 최댓값이 아니라 합으로 갱신한다. 같은 템플릿을 그대로 붙이지 않는다.

> [!example]- 준비 코드 · 전체 구현
>
> 필요한 import와 초기화까지 포함한 전체 코드다. 각 코드 블록은 별도 예시다.
>
> ```java
> class KnapsackTemplate {
>     static long maxValue(int[] weight, int[] value, int capacity) {
>         long[] dp = new long[capacity + 1];
>         for (int i = 0; i < weight.length; i++) {
>             for (int current = capacity; current >= weight[i]; current--) {
>                 dp[current] = Math.max(dp[current],
>                         dp[current - weight[i]] + value[i]);
>             }
>         }
>         return dp[capacity];
>     }
> }
> ```

**관련:** [[DP]] · [[LIS]]
