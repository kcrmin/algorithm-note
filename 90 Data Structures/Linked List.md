---
type: template
aliases: ["Doubly Linked List", "Linked_List", "Doubly_Linked_List"]
---

# Linked List

```java
class Node {
    int value;
    Node prev, next;
    Node(int value) { this.value = value; }
}
Node head = new Node(0), tail = new Node(0);
void init() { head.next = tail; tail.prev = head; }
Node insertAfter(Node previous, int value) {
    Node following = previous.next; // ①
    Node node = new Node(value);
    node.prev = previous;
    node.next = following;
    previous.next = node;
    following.prev = node;
    return node;
}
void remove(Node node) {
    node.prev.next = node.next;     // ②
    node.next.prev = node.prev;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 다음 이웃을 먼저 보관 | previous.next를 바꾼 뒤에도 원래 이웃을 잃지 않는다. |
| 삽입은 연결 4개 | 새 노드의 양쪽과 양 이웃의 방향을 모두 이어야 한다. |
| ② 삭제는 연결 2개 | 삭제할 노드를 건너뛰도록 양 이웃끼리 이어 준다. |

**예시**

```text
삽입 전   previous(10) ↔ following(30)
삽입 후   previous(10) ↔ node(20) ↔ following(30)
삭제      10.next = 30 / 30.prev = 10
더미      head ↔ 실제 원소들 ↔ tail
```

**적용** 노드 위치를 알고 있다면, 이웃의 연결만 바꾸어 O(1)에 삽입·삭제한다.

**준비** head·tail을 한 번 연결하고, 실제 노드만 삭제한다.

**주의** init() 후 사용. tail 뒤 삽입·더미 삭제는 금지. 노드 검색 O(n), 삽입·삭제 O(1), 공간 O(n).

[[Hashing]] · [[Stack and Queue]]
