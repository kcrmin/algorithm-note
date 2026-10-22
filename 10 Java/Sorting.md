---
type: template
tags: ["Java"]
---

# Sorting

```java
// meetings[i]={시작,끝}. 끝 오름차순, 동점이면 시작 오름차순.
Arrays.sort(meetings, (a, b) -> {
    // a-b는 넘치면 비교 부호가 뒤집힐 수 있어 compare를 쓴다.
    int order = Integer.compare(a[1], b[1]);
    if (order != 0) return order; // 첫 기준이 다르면 동점 기준은 보지 않는다.
    return Integer.compare(a[0], b[0]);
});
```

**예시**

```text
[시작, 끝]  [3,5] [1,3] [0,3]
① 끝 오름차순 → 끝 3 두 개가 먼저
② 시작순      → [0,3] [1,3] [3,5]
```

**주의** 음수면 a가 앞, 양수면 b가 앞이다. O(n log n).

[[Two Pointers - Pair Sum]] · [[Greedy]] · [[LIS]] · [[Array Binary Search]]
