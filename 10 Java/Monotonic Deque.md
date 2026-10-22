---
type: template
tags: ["DataStructures"]
aliases: ["Monotonic Deque - Sliding Window Maximum"]
category: two-pointers-windows
categories: [two-pointers-windows]
---

# Monotonic Deque

```text
k = 3, current window = [3  1  2]
candidates (index:value)
front [0:3, 2:2] back     1은 더 늦게 들어온 2에 밀림
          ↑ max = 3

next window = [1  2  5]
0:3 만료 → 2:2 제거 → [3:5] → max = 5
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Expire** | // 현재 창 밖으로 나간 인덱스를 앞에서 버린다. |
| **Prune** | // 새 값 이하의 후보를 뒤에서 버린다. 새 값이 더 늦게 만료되므로 옛 후보가 이길 수 없다. |
| **Push** | // 새 인덱스를 뒤에 넣는다. 후보 값은 앞에서 뒤로 감소한다. |
| **Answer** | // 창이 찼다면 맨 앞의 값이 최댓값이다. |

## Java

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

    if (i >= k - 1) {
        answer[i - k + 1] = arr[deque.peekFirst()];
    }
}
```

길이 k인 모든 창의 최댓값 · `1 ≤ k ≤ arr.length` · `O(n)`

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 앞 / 뒤의 역할 | 앞은 만료 검사와 최댓값 조회, 뒤는 새 값에 밀리는 후보 제거다. |
| 만료 조건 `<= i-k` | 현재 창의 첫 인덱스는 i-k+1이다. 그보다 앞이면 제외한다. |
| 같은 값도 제거 | 새 값이 같으면서 더 늦게 만료되므로 옛 인덱스는 필요 없다. |
| 첫 답은 `i = k-1` | 길이가 k가 되기 전의 부분 창을 답에 넣지 않는다. |
| 최솟값 변형 | 뒤에서 제거하는 비교를 `>=`로 바꾸면 값이 증가하는 후보열이 된다. |

---

**분류** [[00 Maps/Two Pointers and Windows|Two Pointers and Windows]]

**비교** [[20 Ranges/Sliding Window - Fixed Size|Sliding Window - Fixed Size]] · [[10 Java/Monotonic Stack|Monotonic Stack]]
