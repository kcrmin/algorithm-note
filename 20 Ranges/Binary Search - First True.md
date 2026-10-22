---
type: template
tags: ["Ranges"]
aliases: ["Binary_Search", "Binary Search"]
---

# Binary Search - First True

```java
// FFFTTT의 첫 true. valid(x)는 가능 여부, max는 true인 상한.
long left = 0, right = max;
while (left < right) { // 후보가 하나 남으면 더 나눌 필요가 없다.
    long mid = left + (right - left) / 2;
    if (valid(mid)) right = mid; // mid도 첫 true일 수 있어 남긴다.
    else left = mid + 1; // mid와 그 왼쪽은 전부 false라 버린다.
}
return left;
```

**예시**

```text
값     0  1  2  3  4  5
판정   F  F  F  T  T  T
                ↑ 답
mid=2(F) → [3,5] → mid=4(T) → [3,4] → 답 3
```

**주의** 0≤max<Long.MAX_VALUE, 범위 안 T 존재. 검사 비용 × O(log 범위).

[[Binary Search - Last True]] · [[Array Binary Search]] · [[Two Pointers - Pair Sum]] · [[Prefix Sum]]
