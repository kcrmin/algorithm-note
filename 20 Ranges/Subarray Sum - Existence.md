---
type: template
tags: ["Ranges"]
aliases: ["Prefix_Sum_Hash", "Prefix Sum + HashSet", "Prefix Sum + HashMap", "Prefix Sum and HashSet - Existence"]
category: prefix-sums
categories: [prefix-sums]
---

# Subarray Sum - Existence

```text
arr          2  -1   2       target = 3
prefix    0  2   1   3
          ↑          ↑
       이전 합     현재 합

현재 합 - 이전 합 = target
이전 합 = 현재 합 - target = 3 - 3 = 0
seen에 0이 있음 → 합 3인 구간 존재 → true
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Seed** | // 시작 전 합 0을 먼저 저장한다. 배열 처음부터 시작하는 구간도 찾는다. |
| **Expand** | // 현재 원소를 누적 합에 더한다. |
| **Check** | // 필요한 이전 합이 저장되어 있으면 구간이 존재한다. |
| **Save** | // 현재 합을 저장한다. 다음 원소부터 이전 합으로 쓰인다. |

## Java

```java
Set<Long> seen = new HashSet<>();
seen.add(0L);
long sum = 0;

for (int num : arr) {
    sum += num;

    if (seen.contains(sum - target)) {
        return true;
    }

    // 현재 합은 조회 뒤 저장해야 빈 구간을 잡지 않는다.
    seen.add(sum);
}

return false;
```

합=target인 연속 구간 존재 여부 · 음수 허용 · 합·차이 long 범위 · 평균 `O(n)`

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `sum - target` | 현재 합에서 목표 합을 빼면 필요한 이전 합이다. 순서를 반대로 빼지 않는다. |
| 조회 → 저장 | target=0일 때 현재 합을 먼저 저장하면 길이 0인 구간이 자기 자신과 매칭된다. |
| 시작 전 `0` | 예시의 첫 원소부터 끝까지인 구간은 이전 합 0이 있어야 찾는다. |
| Map과 Set | 존재 여부는 Set, 개수는 Map의 등장 횟수. 같은 누적 합이라도 다른 시작 위치가 될 수 있다. |

---

**분류** [[00 Maps/Prefix Sums|Prefix Sums]]

**비교** [[20 Ranges/Subarray Sum - Count|Subarray Sum - Count]] · [[20 Ranges/Two Pointers - Subarray Sum|Two Pointers - Subarray Sum]]
