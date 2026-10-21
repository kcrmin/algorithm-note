---
type: template
tags: [심화]
aliases: [Knapsack]
---

# 배낭 DP

> **핵심:** 물건을 한 번만 쓰는 0/1 배낭은 용량을 **뒤에서 앞으로** 갱신한다.

## 개념

배낭 DP는 용량 제한 안에서 물건을 선택하는 최적값이나 경우의 수를 상태로 저장하는 방법이다. 아래 0/1 배낭은 각 물건을 최대 한 번만 쓰며, dp[w]는 용량 w 이하에서의 최대 가치다.

## 적용 조건

각 물건의 무게·가치가 있고 용량 안에서 가치 합을 최대로 할 때 쓴다. 무게는 양의 정수, 용량은 0 이상이다. 물건 n개, 용량 W이면 시간 `O(nW)`, 공간 `O(W)`. W가 매우 크면 이 방식은 어렵다.

## Java 템플릿

```java
class KnapsackTemplate {
    static long maxValue(int[] weight, int[] value, int capacity) {
        long[] dp = new long[capacity + 1];
        for (int i = 0; i < weight.length; i++) {
            for (int current = capacity; current >= weight[i]; current--) {
                dp[current] = Math.max(dp[current],
                        dp[current - weight[i]] + value[i]);
            }
        }
        return dp[capacity];
    }
}
```

> [!info]- 원리와 코드 설명
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

**관련 개념:** [[DP]] · [[LIS]]
