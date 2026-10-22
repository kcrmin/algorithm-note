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
    // expand: 오른쪽 한 칸 포함
    sum += arr[right];
    // shrink: 초과하는 동안 왼쪽 제거
    while (left <= right && sum > target) sum -= arr[left++];
    // answer: 빈 구간을 빼고 목표 합 확인
    if (left <= right && sum == target) return true;
}
return false;
```

**조건** 합=target인 연속 구간 존재. 원소·target 모두 양수. O(n).

**분류** [[00 Maps/Two Pointers and Windows|Two Pointers and Windows]]
