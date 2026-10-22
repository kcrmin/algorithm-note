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
    // resolve: 더 큰 값을 만난 대기 인덱스를 모두 해결
    while (!stack.isEmpty() && arr[stack.peek()] < arr[i]) {
        answer[stack.pop()] = i;
    }
    stack.push(i); // wait: 현재 인덱스도 다음 큰 값을 기다림
}
```

**조건** 각 원소 오른쪽의 첫 큰 값 인덱스. 없으면 -1. O(n).

**분류** [[00 Maps/Core Collections|Core Collections]]
