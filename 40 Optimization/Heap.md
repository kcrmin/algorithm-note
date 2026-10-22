---
type: template
aliases: ["Priority Queue", "Priority_Queue"]
---

# Heap

```java
// 최대 Heap이면 생성자를 new PriorityQueue<>(Comparator.reverseOrder())로 바꾼다.
// 부모≤자식이므로 맨 위가 항상 최솟값.
PriorityQueue<Integer> pq = new PriorityQueue<>();
// 넣는 순서와 관계없이 최솟값 후보가 위로 올라간다.
for (int value : arr) pq.offer(value);

while (!pq.isEmpty()) { // 빈 Heap의 poll/peek는 null이므로 먼저 확인한다.
    // 내부 전체는 정렬되어 있지 않다. poll을 반복해야 정렬 순서다.
    int current = pq.poll();
    // 확인만 할 때는 peek(). 새 후보가 생기면 offer()한다.
}
```

**예시**

```text
넣기  3, 1, 2         꺼내기 1
       1                       2
      / \                     /
     3   2                   3
0을 추가하면 다음 꺼낼 값은 0
```

**주의** 빈 Heap의 peek/poll은 null이다. offer·poll O(log n), peek O(1), 공간 O(n).

[[Heap - Lazy Deletion]] · [[Dijkstra]] · [[Stack and Queue]]
