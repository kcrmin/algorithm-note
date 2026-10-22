---
type: template
tags: ["Java"]
category: core-collections
categories: [core-collections]
---

# Stack and Queue

```text
input     10 → 20

Stack     [10, 20] → 20 → 10     나중에 넣은 것부터
Queue     [10, 20] → 10 → 20     먼저 넣은 것부터
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Stack · Push / Pop** | // 같은 쪽 끝에서 넣고 꺼낸다. 가장 최근 항목부터 처리한다. |
| **Queue · Offer / Poll** | // 뒤에 넣고 앞에서 꺼낸다. 들어온 순서대로 처리한다. |
| **Peek** | // 꺼내지 않고 다음으로 처리할 항목만 확인한다. |

## Java

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);
stack.push(20);
int last = stack.pop();

Queue<Integer> queue = new ArrayDeque<>();
queue.offer(10);
queue.offer(20);
int first = queue.poll();
```

`java.util.*` · 양 끝 연산 상환 `O(1)`

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| LIFO / FIFO | Stack은 후입선출, Queue는 선입선출. 위 예시의 last=20, first=10. |
| 빈 `pop()` | 예외가 발생한다. 비어 있을 수 있다면 isEmpty로 먼저 확인한다. |
| 빈 `poll()` / `peek()` | null을 반환한다. int에 바로 대입하면 자동 언박싱에서 예외가 난다. |
| ArrayDeque | null을 저장할 수 없다. Stack 클래스 대신 Deque를 쓰면 같은 구조로 양 끝도 다룰 수 있다. |

---

**분류** [[00 Maps/Core Collections|Core Collections]]
