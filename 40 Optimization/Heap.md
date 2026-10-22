---
type: template
aliases: ["Priority Queue", "Priority_Queue"]
category: core-collections
categories: [core-collections]
---

# Heap

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
for (int value : arr) pq.offer(value);

while (!pq.isEmpty()) { // 빈 Heap의 peek·poll은 null이다.
    int current = pq.poll(); // 최솟값부터 꺼내야 정렬 순서가 된다.
    // current를 처리한다. 조회만 할 때는 pq.peek().
}
```

최대 Heap은 new PriorityQueue<>(Comparator.reverseOrder()) · offer·poll O(log n), peek O(1) · 공간 O(n).

**분류** [[00 Maps/Core Collections|Core Collections]]
