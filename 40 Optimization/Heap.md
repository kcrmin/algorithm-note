---
type: template
aliases: ["Priority Queue", "Priority_Queue"]
---

# Heap

**언제** 값이 계속 추가되는 동안 현재 최솟값을 반복해서 꺼낸다.

```text
넣기  3, 1, 2         꺼내기 1
       1                       2
      / \                     /
     3   2                   3
0을 추가하면 다음 꺼낼 값은 0
```

**코드** Java 기본 PriorityQueue는 최소 Heap이다.

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
for (int value : arr) pq.offer(value);

while (!pq.isEmpty()) {
    int current = pq.poll();
    // current를 처리하고, 새 후보가 생기면 offer한다.
}
```

| 암기 포인트 | 이유 |
|---|---|
| `offer → peek → poll` | 삽입 → 확인 → 제거. peek는 원소를 남겨 둔다. |
| `poll()`만 순서 보장 | 부모 ≤ 자식일 뿐, 내부 배열 전체는 정렬되어 있지 않다. |
| 최대 Heap | 선언의 생성자를 `new PriorityQueue<>(Comparator.reverseOrder())`로 바꾼다. |

**주의** 빈 Heap의 peek/poll은 null이다. offer·poll O(log n), peek O(1), 공간 O(n).

[[Heap - Lazy Deletion]] · [[Dijkstra]] · [[Stack and Queue]]
