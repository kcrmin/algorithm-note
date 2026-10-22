---
type: template
tags: ["DataStructures"]
aliases: ["Monotonic Deque - Sliding Window Maximum"]
---

# Monotonic Deque

```java
// 길이 k 창의 최댓값. 1<=k<=arr.length. 값 대신 인덱스를 넣어 만료도 판단한다.
Deque<Integer> deque = new ArrayDeque<>();
int[] answer = new int[arr.length - k + 1];
for (int i = 0; i < arr.length; i++) {
    // 현재 창은 [i-k+1,i]. 그 앞 인덱스는 더 이상 쓸 수 없다.
    while (!deque.isEmpty() && deque.peekFirst() <= i - k) {
        deque.pollFirst();
    }
    // 더 작고 먼저 만료되는 후보는 앞으로도 최댓값이 못 되므로 버린다.
    while (!deque.isEmpty() && arr[deque.peekLast()] <= arr[i]) {
        deque.pollLast();
    }
    deque.offerLast(i);
    // k개가 찬 뒤부터 기록. 남은 값이 큰 순서이므로 맨 앞이 최댓값이다.
    if (i >= k - 1) answer[i - k + 1] = arr[deque.peekFirst()];
}
```

**예시**

```text
arr=[2,1,3], k=2
i=1: Deque 인덱스 [0,1] → 값 [2,1] → 최대2
i=2: 0번은 만료, 1번은 새 값3보다 작아 탈락
     Deque 인덱스 [2]   → 값 [3]   → 최대3
```

**주의** 값 대신 인덱스를 저장한다. 1≤k≤n. 전체 O(n).

[[Sliding Window - Fixed Size]] · [[Monotonic Stack]] · [[Heap]]
