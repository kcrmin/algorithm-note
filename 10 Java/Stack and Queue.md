---
type: template
tags: ["Java"]
---

# Stack and Queue

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);
stack.push(20);
int last = stack.pop();   // 20

Queue<Integer> queue = new ArrayDeque<>();
queue.offer(10);
queue.offer(20);
int first = queue.poll(); // 10
```

| 암기 포인트 | 이유 |
|---|---|
| Stack: `push / pop / peek` | 최근 선택을 먼저 처리하므로 괄호·되돌리기에 맞는다. |
| Queue: `offer / poll / peek` | 들어온 순서대로 처리하므로 BFS의 거리 순서를 지킨다. |

**예시**

```text
넣기  10 → 20 → 30
Stack: 30 → 20 → 10   나중 것부터 (LIFO)
Queue: 10 → 20 → 30   먼저 것부터 (FIFO)
```

**적용** 최근 것을 되돌리면 Stack, 먼저 들어온 것부터 처리하면 Queue.

**준비** `java.util.*`의 ArrayDeque를 쓴다.

**주의** 꺼내기 전 isEmpty(). 빈 pop은 예외, poll은 null. 끝 연산 분할상환 O(1).

[[BFS]] · [[DFS and Graphs]] · [[Monotonic Stack]] · [[Monotonic Deque]]
