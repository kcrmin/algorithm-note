---
type: template
aliases: ["Priority Queue", "Priority_Queue"]
category: core-collections
categories: [core-collections]
---

# Heap

```text
입력: 7, 2, 5, 1       항상 가장 작은 후보부터 꺼내기

        1              poll → 1 → 2 → 5 → 7
      /   \
     2     5           부모 ≤ 자식
    /                  형제끼리의 순서는 보장하지 않음
   7
```

| Step | Pseudocode |
| --- | --- |
| Push | // 처리할 후보들을 우선순위에 맞게 넣는다 |
| Pop | // 현재 남은 후보 중 최우선 하나를 꺼낸다 |
| Process | // 꺼낸 후보를 처리하고, 새 후보가 생기면 다시 넣는다 |

## Java

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();

for (int value : arr) {
    pq.offer(value);
}

while (!pq.isEmpty()) {
    int current = pq.poll();
    // current 처리
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 기본 / 최대 Heap | 기본은 최솟값. 최댓값은 `new PriorityQueue<>(Comparator.reverseOrder())` |
| `poll()` / `peek()` | 꺼내서 제거 / 보기만 함. 빈 큐에서는 둘 다 `null` |
| 순회 / 반복 poll | `for-each`는 정렬 순서가 아니다. 순서대로 처리하려면 계속 꺼낸다 |
| 객체의 우선순위 변경 | 넣은 객체의 필드만 수정하면 Heap 순서가 자동 복구되지 않는다. 새 후보를 넣고 오래된 후보를 건너뛰는 방식 등을 사용 |

`java.util.*` · `offer`, `poll`: `O(log n)` · `peek`: `O(1)` · 공간 `O(n)`.

**분류** [[00 Maps/Core Collections|Core Collections]]
