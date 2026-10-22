---
type: template
tags: ["DataStructures"]
category: core-collections
categories: [core-collections]
---

# Monotonic Stack

```java
int[] answer = new int[arr.length];
Arrays.fill(answer, -1);
Deque<Integer> stack = new ArrayDeque<>();
for (int i = 0; i < arr.length; i++) {
    // 여러 답이 한 번에 정해지므로 while. 같은 값은 제외.
    while (!stack.isEmpty() && arr[stack.peek()] < arr[i]) {
        answer[stack.pop()] = i; // 기다리던 위치의 첫 큰 값이 지금 i다.
    }
    stack.push(i); // 현재 위치도 다음 큰 값을 기다린다.
}
```

**조건** 각 원소 오른쪽의 첫 큰 값 인덱스. 없으면 -1. O(n).

**분류** [[00 Maps/Core Collections|Core Collections]]
