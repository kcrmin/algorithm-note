---
type: template
tags: ["Ranges"]
aliases: ["Prefix Sum and HashMap - Count"]
category: prefix-sums
categories: [prefix-sums]
---

# Subarray Sum - Count

```text
arr = [1, -1, 1]     target = 1

마지막 원소까지 읽은 현재 합 = 1
필요한 이전 합 = 1 - 1 = 0

이전 합:  [0, 1, 0]       0이 두 번 등장
           ↑     ↑
구간:    [1,-1,1] + [1]  → 이번에 +2개
앞서 찾은 첫 [1]까지      → 전체 답 3개
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Seed** | // 시작 전 합 0을 먼저 저장한다. 배열 처음부터 시작하는 구간도 찾는다. |
| **Expand** | // 현재 원소를 누적 합에 더한다. |
| **Count** | // 필요한 이전 합이 등장한 횟수를 답에 더한다. |
| **Save** | // 현재 합을 저장한다. 다음 원소부터 이전 합으로 쓰인다. |

## Java

```java
Map<Long, Long> count = new HashMap<>();
count.put(0L, 1L);

long sum = 0;
long answer = 0;

for (int num : arr) {
    sum += num;
    answer += count.getOrDefault(sum - target, 0L);

    // 현재 합은 조회 뒤 저장해야 빈 구간을 세지 않는다.
    count.put(sum, count.getOrDefault(sum, 0L) + 1);
}

return answer;
```

합=target인 연속 구간 개수 · 음수 허용 · 합·차이 long 범위 · 평균 `O(n)`

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `sum - target` | 현재 합에서 목표 합을 빼면 필요한 이전 합이다. 순서를 반대로 빼지 않는다. |
| 조회 → 저장 | target=0일 때 현재 합을 먼저 저장하면 길이 0인 구간이 자기 자신과 매칭된다. |
| 시작 전 `0` | 예시의 첫 원소부터 끝까지인 구간은 이전 합 0이 있어야 찾는다. |
| Map과 Set | 존재 여부는 Set, 개수는 Map의 등장 횟수. 같은 누적 합이라도 다른 시작 위치가 될 수 있다. |

---

**분류** [[00 Maps/Prefix Sums|Prefix Sums]]
