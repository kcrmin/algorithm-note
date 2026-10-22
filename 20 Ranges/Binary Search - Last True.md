---
type: template
tags: ["Ranges"]
category: binary-search
categories: [binary-search]
---

# Binary Search - Last True

```java
long left = 0, right = max;
while (left < right) {
    long mid = left + (right - left + 1) / 2; // mid: 위쪽을 골라 left=mid도 전진
    if (valid(mid)) left = mid; // true: mid를 남기고 더 오른쪽 탐색
    else right = mid - 1; // false: mid부터 F이므로 버림
}
return left; // answer: 마지막 T
```

**조건** TTTFFF의 마지막 T. valid(0)=true, 0≤max<Long.MAX_VALUE. 검사×O(log 범위).

**분류** [[00 Maps/Binary Search|Binary Search]]
