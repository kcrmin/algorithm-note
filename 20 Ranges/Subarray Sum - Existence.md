---
type: template
tags: ["Ranges"]
aliases: ["Prefix_Sum_Hash", "Prefix Sum + HashSet", "Prefix Sum + HashMap", "Prefix Sum and HashSet - Existence"]
---

# Subarray Sum - Existence

```java
// 음수도 가능. 합이 target인 비어 있지 않은 연속 구간을 찾는다.
Set<Long> seen = new HashSet<>();
seen.add(0L); // 읽기 전 합 0도 시작점 하나여야 첫 원소부터인 구간을 찾는다.
long sum = 0;
for (int num : arr) {
    sum += num;
    // 현재 합-이전 합=target이므로, 이전 합 sum-target을 찾는다.
    if (seen.contains(sum - target)) return true;
    // 먼저 저장하면 target=0일 때 자기 자신과의 차이인 빈 구간을 잡는다.
    seen.add(sum);
}
return false;
```

**예시**

```text
이전 합 A ──── [찾는 구간] ──── 현재 합 B
                    B − A = target
찾을 이전 합 A = B − target
[3,-2,4], target=2: 현재합5 − target2 = 이전합3 ✓
```

**주의** 존재만 찾는다. 개수는 HashMap. 평균 시간·공간 O(n), 합은 long.

[[Subarray Sum - Count]] · [[Prefix Sum]] · [[Hashing]] · [[Two Pointers - Subarray Sum]]
