---
type: template
tags: ["Ranges"]
aliases: ["Prefix Sum and HashMap - Count"]
category: prefix-sums
categories: [prefix-sums]
---

# Subarray Sum - Count

```java
Map<Long, Long> count = new HashMap<>();
// seed: 시작 전 합 0도 한 번 등장
count.put(0L, 1L);
long sum = 0, answer = 0;
for (int num : arr) {
    sum += num;
    // count: sum-이전합=target인 이전 합의 횟수
    answer += count.getOrDefault(sum - target, 0L);
    // save: 현재 합은 나중에 저장, 빈 구간 제외
    count.put(sum, count.getOrDefault(sum, 0L) + 1);
}
return answer;
```

**조건** 합=target인 연속 구간 개수. 음수 허용, 합·차이 long 범위. 평균 O(n).

**분류** [[00 Maps/Prefix Sums|Prefix Sums]]
