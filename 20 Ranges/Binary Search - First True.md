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
    if (valid(mid)) right = mid; // mid도 첫 T일 수 있어 남긴다.
    else left = mid + 1; // mid와 그 왼쪽은 전부 F라 버린다.
}
return left;
```

**조건** FFFTTT의 첫 T. valid(max)=true, 0≤max<Long.MAX_VALUE. 검사×O(log 범위).

**분류** [[00 Maps/Binary Search|Binary Search]]

**비교** [[20 Ranges/Binary Search - Last True|Binary Search - Last True]]
