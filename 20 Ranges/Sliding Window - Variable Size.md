---
type: template
tags: ["Ranges"]
category: two-pointers-windows
categories: [two-pointers-windows]
---

# Sliding Window - Variable Size

```java
int left = 0, minLength = Integer.MAX_VALUE;
long sum = 0;
for (int right = 0; right < arr.length; right++) {
    sum += arr[right];
    while (sum >= target) { // 여전히 만족하면 더 짧은 창도 확인한다.
        // 빼면 후보가 사라질 수 있어 먼저 기록.
        minLength = Math.min(minLength, right - left + 1);
        sum -= arr[left++];
    }
}
return minLength == Integer.MAX_VALUE ? 0 : minLength;
```

**조건** 합≥target인 최단 구간, 없으면 0. 원소·target 모두 양수. O(n).

**분류** [[00 Maps/Two Pointers and Windows|Two Pointers and Windows]]

**비교** [[20 Ranges/Two Pointers - Subarray Sum|Two Pointers - Subarray Sum]]
