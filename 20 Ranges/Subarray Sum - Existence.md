---
type: template
tags: ["Ranges"]
aliases: ["Prefix_Sum_Hash", "Prefix Sum + HashSet", "Prefix Sum + HashMap", "Prefix Sum and HashSet - Existence"]
---

# Subarray Sum - Existence

```java
Set<Long> seen = new HashSet<>();
seen.add(0L);
long sum = 0;
for (int num : arr) {
    sum += num;
    if (seen.contains(sum - target)) return true;  // ①
    seen.add(sum);                               // ②
}
return false;
```

| 암기 포인트 | 이유 |
|---|---|
| `seen.add(0L)` | 읽기 전 합 0도 시작점 하나다. 첫 원소부터의 구간을 찾게 한다. |
| ① `contains(sum−target)` | 현재 합과의 차이가 target인 이전 합을 찾는다. |
| ② 확인한 뒤 `add(sum)` | 먼저 넣으면 target=0일 때 길이 0인 구간을 답으로 잡는다. |

**예시**

```text
이전 합 A ──── [찾는 구간] ──── 현재 합 B
                    B − A = target
찾을 이전 합 A = B − target
[3,-2,4], target=2: 현재합5 − target2 = 이전합3 ✓
```

**적용** 음수가 섞여 있어도 합이 target인 연속 구간이 하나라도 있는지 찾는다.

**준비** `arr`, `target`을 받는 boolean 메서드 내부. `java.util.*`.

**주의** 존재만 찾는다. 개수는 HashMap. 평균 시간·공간 O(n), 합은 long.

[[Subarray Sum - Count]] · [[Prefix Sum]] · [[Hashing]] · [[Two Pointers - Subarray Sum]]
