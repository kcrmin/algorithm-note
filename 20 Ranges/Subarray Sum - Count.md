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
count.put(0L, 1L); // 시작 전 합 0이 한 번 있었다.
long sum = 0, answer = 0;
for (int num : arr) {
    sum += num;
    // sum-이전합=target. 이전 합 횟수만큼 답이 늘어난다.
    answer += count.getOrDefault(sum - target, 0L);
    // 현재 합은 나중에 세어 빈 구간을 제외.
    count.put(sum, count.getOrDefault(sum, 0L) + 1);
}
return answer;
```

**조건** 합=target인 연속 구간 개수. 음수 허용, 합·차이 long 범위. 평균 O(n).

**분류** [[00 Maps/Prefix Sums|Prefix Sums]]
