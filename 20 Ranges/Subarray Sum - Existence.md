---
type: template
tags: ["Ranges"]
aliases: ["Prefix_Sum_Hash", "Prefix Sum + HashSet", "Prefix Sum + HashMap", "Prefix Sum and HashSet - Existence"]
category: prefix-sums
categories: [prefix-sums]
---

# Subarray Sum - Existence

```java
Set<Long> seen = new HashSet<>();
seen.add(0L); // 처음부터인 구간도 찾도록 시작 전 합을 넣는다.
long sum = 0;
for (int num : arr) {
    sum += num;
    // 현재 합-이전 합=target인 이전 합을 찾는다.
    if (seen.contains(sum - target)) return true;
    seen.add(sum); // 확인 후 저장해야 빈 구간을 잡지 않는다.
}
return false;
```

**조건** 합=target인 연속 구간 존재. 음수 허용, 합·차이 long 범위. 평균 O(n).

**분류** [[00 Maps/Prefix Sums|Prefix Sums]]

**비교** [[20 Ranges/Subarray Sum - Count|Subarray Sum - Count]] · [[20 Ranges/Two Pointers - Subarray Sum|Two Pointers - Subarray Sum]]
