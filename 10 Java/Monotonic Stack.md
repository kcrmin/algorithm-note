---
type: template
tags: ["DataStructures"]
---

# Monotonic Stack

```java
int[] answer = new int[arr.length];
Arrays.fill(answer, -1);
Deque<Integer> stack = new ArrayDeque<>();
for (int i = 0; i < arr.length; i++) {
    while (!stack.isEmpty() && arr[stack.peek()] < arr[i]) {
        answer[stack.pop()] = i;
    }
    stack.push(i);
}
```

| 암기 포인트 | 이유 |
|---|---|
| `while (arr[top] < arr[i])` | 한 번에 여러 원소의 답이 확정되므로 while이다. |
| `answer[pop()] = i` | 대기 중인 위치에 지금 찾은 첫 큰 값의 위치를 쓴다. |
| `<`, 그리고 마지막에 push | 같은 값은 큰 값이 아니다. 현재 원소는 이전 답을 정한 뒤 대기한다. |

**예시**

```text
index  0  1  2
arr    2  1  3
       └─────↑  3이 오면 2와 1의 답이 동시 확정
answer 2  2 -1  ← 큰 값 자체가 아니라 위치
```

**적용** 각 원소 오른쪽의 첫 큰 값 위치를 찾는다. 아직 답이 없는 인덱스를 쌓는다.

**준비** `arr`는 int 배열. `Arrays`, `Deque`는 `java.util.*`.

**주의** 못 찾으면 −1. 각 인덱스가 한 번 들어오고 나가 전체 O(n).

[[Monotonic Deque]] · [[Stack and Queue]]
