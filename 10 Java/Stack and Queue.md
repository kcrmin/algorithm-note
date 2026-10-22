---
type: template
tags: ["Java"]
---

# Stack and Queue

```java
// ArrayDeque로 Stack과 Queue를 만든다. 확인만 할 때는 peek로 제거하지 않는다.
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);
stack.push(20);
int last = stack.pop(); // 마지막에 넣은 20부터 꺼내므로 되돌리기에 맞는다.

Queue<Integer> queue = new ArrayDeque<>();
queue.offer(10);
queue.offer(20);
int first = queue.poll(); // 먼저 넣은 10부터 꺼내므로 BFS의 거리 순서를 지킨다.
```

**예시**

```text
넣기  10 → 20 → 30
Stack: 30 → 20 → 10   나중 것부터 (LIFO)
Queue: 10 → 20 → 30   먼저 것부터 (FIFO)
```

**주의** 꺼내기 전 isEmpty(). 빈 pop은 예외, poll은 null. 끝 연산 분할상환 O(1).

[[BFS]] · [[DFS and Graphs]] · [[Monotonic Stack]] · [[Monotonic Deque]]
