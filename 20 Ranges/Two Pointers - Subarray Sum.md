---
type: template
tags: ["Ranges"]
category: two-pointers-windows
categories: [two-pointers-windows]
---

# Two Pointers - Subarray Sum

```java
int left = 0;
long sum = 0;
for (int right = 0; right < arr.length; right++) {
    sum += arr[right];
    // 초과할 때만 줄인다. 여러 개를 뺄 수 있어 while.
    while (left <= right && sum > target) sum -= arr[left++];
    // 빈 구간(left>right)은 답에서 제외.
    if (left <= right && sum == target) return true;
}
return false;
```

**조건** 합=target인 연속 구간 존재. 원소·target 모두 양수. O(n).

**분류** [[00 Maps/Two Pointers and Windows|Two Pointers and Windows]]
