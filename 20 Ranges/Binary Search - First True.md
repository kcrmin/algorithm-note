---
type: template
tags: ["Ranges"]
aliases: ["Binary_Search", "Binary Search"]
category: binary-search
categories: [binary-search]
---

# Binary Search - First True

```java
long left = 0, right = max;
while (left < right) {
    long mid = left + (right - left) / 2;
    if (valid(mid)) right = mid; // true: mid를 남기고 더 왼쪽 탐색
    else left = mid + 1; // false: mid까지 F이므로 버림
}
return left; // answer: 첫 T
```

**조건** FFFTTT의 첫 T. valid(max)=true, 0≤max<Long.MAX_VALUE. 검사×O(log 범위).

**분류** [[00 Maps/Binary Search|Binary Search]]

**비교** [[20 Ranges/Binary Search - Last True|Binary Search - Last True]]
