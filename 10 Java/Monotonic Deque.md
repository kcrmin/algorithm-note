---
type: template
tags: ["DataStructures"]
aliases: ["Monotonic Deque - Sliding Window Maximum"]
category: two-pointers-windows
categories: [two-pointers-windows]
---

# Monotonic Deque

```java
Deque<Integer> deque = new ArrayDeque<>();
int[] answer = new int[arr.length - k + 1];
for (int i = 0; i < arr.length; i++) {
    // expire: 현재 창 [i-k+1,i] 밖의 인덱스 제거
    while (!deque.isEmpty() && deque.peekFirst() <= i - k) {
        deque.pollFirst();
    }
    // prune: 새 값 이하이고 먼저 만료될 옛 후보 제거
    while (!deque.isEmpty() && arr[deque.peekLast()] <= arr[i]) {
        deque.pollLast();
    }
    deque.offerLast(i);
    // answer: 값이 내림차순으로 남아 맨 앞이 창의 최댓값
    if (i >= k - 1) answer[i - k + 1] = arr[deque.peekFirst()];
}
```

**조건** 길이 k 구간 최댓값. 1≤k≤arr.length. O(n).

**분류** [[00 Maps/Two Pointers and Windows|Two Pointers and Windows]]

**비교** [[20 Ranges/Sliding Window - Fixed Size|Sliding Window - Fixed Size]] · [[10 Java/Monotonic Stack|Monotonic Stack]]
