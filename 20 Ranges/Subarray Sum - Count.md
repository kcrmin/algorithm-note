---
type: template
tags: ["Ranges"]
aliases: ["Prefix Sum and HashMap - Count"]
---

# Subarray Sum - Count

**언제** 음수·0이 있어도 합이 target인 연속 구간의 개수를 센다.

```text
arr=[1,-1,1], target=1, 시작 전 count={0:1}
현재 합     1     0     1
필요한 합   0    -1     0
이전 등장   1회   0회   2회  → 답 3
마지막의 0 두 번 = 시작 전 합0 + [1,-1]의 합0
```

**코드** `arr`, `target`을 받는 long 메서드 내부. `java.util.*`.

```java
Map<Long, Long> count = new HashMap<>();
count.put(0L, 1L);
long sum = 0, answer = 0;
for (int num : arr) {
    sum += num;
    answer += count.getOrDefault(sum - target, 0L);  // ①
    count.put(sum, count.getOrDefault(sum, 0L) + 1);  // ②
}
return answer;
```

| 암기 포인트 | 이유 |
|---|---|
| `count.put(0L, 1L)` | 읽기 전 합 0이 한 번 있었다. 배열 처음부터인 구간도 센다. |
| ① 이전 합의 횟수를 더함 | 같은 이전 합이 두 번이면 서로 다른 시작점 두 개다. |
| ② 현재 합은 나중에 저장 | 현재 위치를 시작점으로 쓰는 빈 구간을 제외한다. |

**주의** 답은 n(n+1)/2까지 커져 answer도 long. 평균 시간·공간 O(n).

[[Subarray Sum - Existence]] · [[Prefix Sum]] · [[Hashing]]
