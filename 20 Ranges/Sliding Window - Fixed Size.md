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
long sum = 0, max = Long.MIN_VALUE; // 전부 음수여도 0을 답으로 고르지 않는다.
for (int right = 0; right < arr.length; right++) {
    sum += arr[right];
    // 한 칸씩 넣으므로 초과도 한 칸뿐.
    if (right - left + 1 > k) sum -= arr[left++];
    // 덜 찬 창은 후보가 아니다.
    if (right - left + 1 == k) max = Math.max(max, sum);
}
return max;
```

**조건** 길이 k의 최대 합. 1≤k≤arr.length, 음수 허용. O(n).

**분류** [[00 Maps/Two Pointers and Windows|Two Pointers and Windows]]

**비교** [[20 Ranges/Sliding Window - Variable Size|Sliding Window - Variable Size]]
