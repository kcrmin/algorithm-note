---
type: template
tags: ["Java"]
---

# Sorting

```java
Arrays.sort(meetings, (a, b) -> {
    int order = Integer.compare(a[1], b[1]);
    if (order != 0) return order;
    return Integer.compare(a[0], b[0]);
});
```

| 암기 포인트 | 이유 |
|---|---|
| 첫 비교가 0이 아니면 반환 | 끝 시간이 다르면 두 번째 기준을 볼 필요가 없다. |
| `Integer.compare(a, b)` | a−b는 int가 넘치면 비교 부호가 뒤집힌다. |

**예시**

```text
[시작, 끝]  [3,5] [1,3] [0,3]
① 끝 오름차순 → 끝 3 두 개가 먼저
② 시작순      → [0,3] [1,3] [3,5]
```

**적용** 두 기준으로 정렬한다. 예: 회의를 끝 시간순, 동점이면 시작 시간순.

**준비** `meetings[i] = {시작, 끝}`인 int[][].

**주의** 음수면 a가 앞, 양수면 b가 앞이다. O(n log n).

[[Two Pointers - Pair Sum]] · [[Greedy]] · [[LIS]] · [[Array Binary Search]]
