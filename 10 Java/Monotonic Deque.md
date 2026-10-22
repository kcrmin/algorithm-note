---
type: template
tags: ["DataStructures"]
aliases: ["Monotonic Deque - Sliding Window Maximum"]
---

# Monotonic Deque

**언제** 길이 k 창을 옮기며 최댓값을 매번 구한다. 큰 후보만 Deque에 남긴다.

```text
arr=[2,1,3], k=2
i=1: Deque 인덱스 [0,1] → 값 [2,1] → 최대2
i=2: 0번은 만료, 1번은 새 값3보다 작아 탈락
     Deque 인덱스 [2]   → 값 [3]   → 최대3
```

**코드** `arr`는 int 배열. Deque는 `java.util.*`.

```java
Deque<Integer> deque = new ArrayDeque<>();
int[] answer = new int[arr.length - k + 1];
for (int i = 0; i < arr.length; i++) {
    while (!deque.isEmpty() && deque.peekFirst() <= i - k) {
        deque.pollFirst();
    }
    while (!deque.isEmpty() && arr[deque.peekLast()] <= arr[i]) {
        deque.pollLast();
    }
    deque.offerLast(i);
    if (i >= k - 1) answer[i - k + 1] = arr[deque.peekFirst()];
}
```

| 암기 포인트 | 이유 |
|---|---|
| 앞: `index <= i-k` 제거 | 현재 창 [i−k+1,i]를 벗어난 후보는 쓸 수 없다. |
| 뒤: `value <= arr[i]` 제거 | 더 작고 먼저 만료될 값은 앞으로도 최댓값이 못 된다. |
| `peekFirst()` | 남은 값이 큰 순서라 맨 앞이 현재 최댓값이다. |

**주의** 값 대신 인덱스를 저장한다. 1≤k≤n. 전체 O(n).

[[Sliding Window - Fixed Size]] · [[Monotonic Stack]] · [[Heap]]
