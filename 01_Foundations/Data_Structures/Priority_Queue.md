---
type: data-structure
tags:
  - priority-queue
  - heap
language: java
status: in-review
source: []
---

# Priority Queue
> **한 줄 요약:** 우선순위가 가장 높은(또는 낮은) 원소를 빠르게 꺼내기 위한 힙 기반 자료구조

## Core Concept
* **When to use:**
  - 가장 작은/큰 값을 반복적으로 꺼내며 처리할 때
  - "가장 유리한 후보"를 매 단계 선택해야 할 때 (예: Dijkstra, 작업 스케줄링)
* **Key Point:**
  - 최소 힙: 가장 작은 값이 우선 (기본)
  - 최대 힙: 가장 큰 값이 우선 (Comparator 필요)
  - 내부적으로 완전 이진 트리로 구현, top만 정렬 보장

## API & State Change (if applicable)
| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Init (Min-Heap) | `new PriorityQueue<>();` | `pq` 초기화 | 기본은 최소 힙 |
| Init (Max-Heap) | `new PriorityQueue<>(Collections.reverseOrder());` | `pq` 초기화 | 최대 힙 |
| Add/Update | `pq.add(x);` | 상태 변경 | 원소 삽입 |
| Access | `pq.peek();` | top 반환 | 최우선 원소 확인 |
| Delete | `pq.poll();` | top 반환 + 상태 변경 | 최우선 원소 제거/반환 |
| Size/Empty | `pq.size()` / `pq.isEmpty()` | 값 반환 | 크기/비었는지 확인 |

## Patterns
### Pattern A: Min-Heap 기본 사용 (가장 작은 값 반복 처리)
- 최소값을 계속 꺼내면서 처리
```java
PriorityQueue<Integer> pq = new PriorityQueue<>();

pq.add(3);
pq.add(1);
pq.add(5);

while (!pq.isEmpty()) {
    int cur = pq.poll();   // 항상 현재 최소값
    // cur 처리
}
```

### Pattern B: 커스텀 우선순위 (cost 기준 최소 힙)
- (state, cost) 중 cost가 작은 것부터 꺼냄 (Dijkstra 전형)
```java
static class Node {
    int v, cost;
    Node(int v, int cost) { this.v = v; this.cost = cost; }
}

Queue<Node> pq =
    new PriorityQueue<>(Comparator.comparingInt(n -> n.cost));

pq.add(new Node(0, 10));
pq.add(new Node(1, 3));
pq.add(new Node(2, 7));

while (!pq.isEmpty()) {
    Node cur = pq.poll(); // cost가 가장 작은 노드
    // cur 처리
}
```

## Pitfalls & Tips
- PriorityQueue는 "전체가 정렬된 컨테이너"가 아니다 (top만 보장)
- 커스텀 객체는 반드시 Comparator 제공 필요
