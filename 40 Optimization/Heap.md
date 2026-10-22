---
type: template
aliases: ["Priority Queue", "Priority_Queue"]
category: core-collections
categories: [core-collections]
---

# Heap

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
// push: 후보 넣기
for (int value : arr) pq.offer(value);
while (!pq.isEmpty()) {
    // pop: 남은 후보 중 최솟값 꺼내기
    int current = pq.poll();
    // process: current 처리, 확인만 할 때는 pq.peek()
}
```

최대 Heap은 new PriorityQueue<>(Comparator.reverseOrder()) · offer·poll O(log n), peek O(1) · 공간 O(n).

**분류** [[00 Maps/Core Collections|Core Collections]]
