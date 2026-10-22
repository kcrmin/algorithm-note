---
type: template
tags: ["Ranges"]
aliases: ["Two_Pointers", "Two Pointers"]
category: two-pointers-windows
categories: [two-pointers-windows]
---

# Two Pointers - Pair Sum

```java
int left = 0, right = arr.length - 1;
while (left < right) { // 같은 원소를 두 번 쓰지 않는다.
    long sum = (long) arr[left] + arr[right];
    if (sum == target) return true;
    if (sum < target) left++; // 지금 left는 최대 right와도 부족하다.
    else right--; // 지금 right는 최소 left와도 너무 크다.
}
return false;
```

**조건** arr 오름차순, 서로 다른 두 원소의 합=target. O(n), 정렬 별도.

**분류** [[00 Maps/Two Pointers and Windows|Two Pointers and Windows]]
