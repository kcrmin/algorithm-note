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
    // [3,4]에서 4를 골라야 left=mid도 전진한다.
    long mid = left + (right - left + 1) / 2;
    if (valid(mid)) left = mid; // mid는 성공이므로 남기고 더 큰 답을 찾는다.
    else right = mid - 1; // mid부터 오른쪽은 전부 실패다.
}
return left;
```

**조건** TTTFFF의 마지막 T. valid(0)=true, 0≤max<Long.MAX_VALUE. 검사×O(log 범위).

**분류** [[00 Maps/Binary Search|Binary Search]]
