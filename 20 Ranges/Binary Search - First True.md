---
type: template
tags: ["Ranges"]
aliases: ["Binary_Search", "Binary Search"]
---

# Binary Search - First True

```java
long left = 0, right = max;
while (left < right) {
    long mid = left + (right - left) / 2;
    if (valid(mid)) right = mid;  // ①
    else left = mid + 1;         // ②
}
return left;
```

| 암기 포인트 | 이유 |
|---|---|
| ① T는 포함: `right = mid` | mid 자체가 첫 T일 수 있다. |
| ② F는 제외: `left = mid+1` | mid와 그 왼쪽은 전부 F다. |
| `left == right`이면 끝 | 답 후보가 하나 남았으므로 그 값이 답이다. |

**예시**

```text
값     0  1  2  3  4  5
판정   F  F  F  T  T  T
                ↑ 답
mid=2(F) → [3,5] → mid=4(T) → [3,4] → 답 3
```

**적용** 후보가 커질수록 F→T로 바뀔 때 첫 T. 범위 끝 max는 반드시 T.

**준비** `valid(x)`는 문제의 가능 여부를 반환하는 메서드.

**주의** 0≤max<Long.MAX_VALUE, 범위 안 T 존재. 검사 비용 × O(log 범위).

[[Binary Search - Last True]] · [[Array Binary Search]] · [[Two Pointers - Pair Sum]] · [[Prefix Sum]]
