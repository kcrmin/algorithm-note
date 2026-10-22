---
type: template
tags: ["Ranges"]
aliases: ["Sliding_Window", "Sliding Window"]
category: two-pointers-windows
categories: [two-pointers-windows]
---

# Sliding Window - Fixed Size

```java
int left = 0;
long sum = 0, max = Long.MIN_VALUE;
for (int right = 0; right < arr.length; right++) {
    // expand: 오른쪽 한 칸 포함
    sum += arr[right];
    // shrink: k개 초과면 왼쪽 한 칸 제거
    if (right - left + 1 > k) sum -= arr[left++];
    // answer: k개가 찼을 때만 갱신
    if (right - left + 1 == k) max = Math.max(max, sum);
}
return max;
```

**조건** 길이 k의 최대 합. 1≤k≤arr.length, 음수 허용. O(n).

**분류** [[00 Maps/Two Pointers and Windows|Two Pointers and Windows]]

**비교** [[20 Ranges/Sliding Window - Variable Size|Sliding Window - Variable Size]]
