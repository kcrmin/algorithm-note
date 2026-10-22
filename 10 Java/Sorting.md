---
type: template
tags: ["Java"]
category: java-essentials
categories: [java-essentials]
---

# Sorting

```java
Arrays.sort(meetings, (a, b) -> {
    int order = Integer.compare(a[1], b[1]); // a-b는 넘치면 부호가 뒤집힌다.
    if (order != 0) return order; // 첫 기준이 다르면 동점 기준을 보지 않는다.
    return Integer.compare(a[0], b[0]);
});
```

**조건** meetings[i]={시작,끝}. 끝↑, 동점이면 시작↑. O(n log n).

**분류** [[00 Maps/Java Essentials|Java Essentials]]

**비교** [[90 Data Structures/Frequency Array and Buckets|Frequency Array and Buckets]]
