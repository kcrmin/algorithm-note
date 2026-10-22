---
type: template
tags: ["Java"]
category: core-collections
categories: [core-collections]
---

# Stack and Queue

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);
stack.push(20);
int last = stack.pop(); // 나중에 넣은 20부터: LIFO.
Queue<Integer> queue = new ArrayDeque<>();
queue.offer(10);
queue.offer(20);
int first = queue.poll(); // 먼저 넣은 10부터: FIFO.
```

**조건** ArrayDeque 사용. 빈 pop은 예외, poll은 null. 끝 연산 O(1) 상환.

**분류** [[00 Maps/Core Collections|Core Collections]]
