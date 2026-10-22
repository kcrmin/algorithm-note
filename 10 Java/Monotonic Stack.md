---
type: template
tags: ["DataStructures"]
---

# Monotonic Stack

```java
// 각 원소 오른쪽의 첫 큰 값 인덱스. Stack에는 아직 답 없는 인덱스를 둔다.
int[] answer = new int[arr.length];
Arrays.fill(answer, -1); // 끝까지 큰 값을 못 만나면 -1로 남는다.
Deque<Integer> stack = new ArrayDeque<>();
for (int i = 0; i < arr.length; i++) {
    // 여러 답이 한꺼번에 정해질 수 있어 while. 같은 값은 큰 값이 아니라 <.
    while (!stack.isEmpty() && arr[stack.peek()] < arr[i]) {
        // 기다리던 원소가 처음 만난 큰 값의 위치가 지금 i다.
        answer[stack.pop()] = i;
    }
    stack.push(i); // 현재 원소도 다음 큰 값을 기다리므로 마지막에 넣는다.
}
```

**예시**

```text
index  0  1  2
arr    2  1  3
       └─────↑  3이 오면 2와 1의 답이 동시 확정
answer 2  2 -1  ← 큰 값 자체가 아니라 위치
```

**주의** 못 찾으면 −1. 각 인덱스가 한 번 들어오고 나가 전체 O(n).

[[Monotonic Deque]] · [[Stack and Queue]]
