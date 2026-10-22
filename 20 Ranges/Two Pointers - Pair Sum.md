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
while (left < right) {
    long sum = (long) arr[left] + arr[right];
    if (sum == target) return true; // check: 양끝 합이 목표인가
    if (sum < target) left++; // increase: 가장 큰 right와 더해도 작으니 left 제외
    else right--; // decrease: 가장 작은 left와 더해도 크니 right 제외
}
return false;
```

**조건** arr 오름차순, 서로 다른 두 원소의 합=target. O(n), 정렬 별도.

**분류** [[00 Maps/Two Pointers and Windows|Two Pointers and Windows]]
