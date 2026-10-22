---
type: template
tags: ["Ranges"]
---

# Binary Search - Last True

```java
// TTTFFF의 마지막 true. valid(x)는 가능 여부, valid(0)는 true.
long left = 0, right = max;
while (left < right) {
    // [3,4]에서 4를 골라야 left=mid 뒤에도 범위가 줄어든다.
    long mid = left + (right - left + 1) / 2;
    if (valid(mid)) left = mid; // mid는 성공이므로 남기고 더 큰 답을 찾는다.
    else right = mid - 1; // mid부터 오른쪽은 전부 실패다.
}
return left;
```

**예시**

```text
값     0  1  2  3  4  5
판정   T  T  T  T  F  F
                ↑ 답
[3,4]에서는 mid=4여야 left=mid 뒤에도 범위가 줄어든다.
```

**주의** 0≤max<Long.MAX_VALUE, valid(0)=true. 검사 비용 × O(log 범위).

[[Binary Search - First True]] · [[Array Binary Search]]
