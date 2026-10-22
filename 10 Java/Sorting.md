---
type: template
tags: ["Java"]
category: java-essentials
categories: [java-essentials]
---

# Sorting

```java
Arrays.sort(meetings, (a, b) -> {
    int order = Integer.compare(a[1], b[1]); // primary: 종료 시각순; 뺄셈 비교는 overflow 위험
    if (order != 0) return order;
    return Integer.compare(a[0], b[0]); // tie: 같으면 시작 시각순
});
```

**조건** meetings[i]={시작,끝}. 끝↑, 동점이면 시작↑. O(n log n).

**분류** [[00 Maps/Java Essentials|Java Essentials]]

**비교** [[90 Data Structures/Frequency Array and Buckets|Frequency Array and Buckets]]
