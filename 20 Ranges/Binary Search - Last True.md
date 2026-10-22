---
type: template
tags: ["Ranges"]
---

# Binary Search - Last True

**언제** 후보가 커질수록 T→F로 바뀔 때 마지막 T. 시작 0은 반드시 T.

```text
값     0  1  2  3  4  5
판정   T  T  T  T  F  F
                ↑ 답
[3,4]에서는 mid=4여야 left=mid 뒤에도 범위가 줄어든다.
```

**코드** `valid(x)`는 문제의 가능 여부를 반환하는 메서드.

```java
long left = 0, right = max;
while (left < right) {
    long mid = left + (right - left + 1) / 2;  // ①
    if (valid(mid)) left = mid;              // ②
    else right = mid - 1;
}
return left;
```

| 암기 포인트 | 이유 |
|---|---|
| ① mid의 `+1` | 두 후보일 때 오른쪽을 검사해야 같은 자리에서 반복하지 않는다. |
| ② T는 포함: `left = mid` | 더 큰 T를 찾되 현재 성공도 남긴다. |
| F는 제외: `right = mid−1` | mid부터 오른쪽은 전부 F다. |

**주의** 0≤max<Long.MAX_VALUE, valid(0)=true. 검사 비용 × O(log 범위).

[[Binary Search - First True]] · [[Array Binary Search]]
