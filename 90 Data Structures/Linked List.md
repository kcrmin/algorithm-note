---
type: template
aliases: ["Doubly Linked List", "Linked_List", "Doubly_Linked_List"]
---

# Linked List

```java
// init() 후 사용. 이전 노드 위치를 알고 있을 때 삽입·삭제 O(1), 노드 검색은 O(n).
class Node {
    int value;
    Node prev, next;
    Node(int value) { this.value = value; }
}
// 더미 노드가 양 끝의 이웃이 되어 경계 분기를 없앤다.
Node head = new Node(0), tail = new Node(0);
void init() { head.next = tail; tail.prev = head; }
Node insertAfter(Node previous, int value) { // previous는 tail이 아니어야 한다.
    // previous.next를 바꾸기 전에 원래 오른쪽 이웃을 보관한다.
    Node following = previous.next;
    Node node = new Node(value);
    node.prev = previous;
    node.next = following;
    // 새 노드 양쪽과 양 이웃을 모두 연결해야 양방향 순회가 유지된다.
    previous.next = node;
    following.prev = node;
    return node;
}
void remove(Node node) { // 연결된 실제 노드만 삭제. head·tail은 삭제하지 않는다.
    node.prev.next = node.next; // 양 이웃이 삭제 노드를 건너뛰도록 서로 연결한다.
    node.next.prev = node.prev;
}
```

**예시**

```text
삽입 전   previous(10) ↔ following(30)
삽입 후   previous(10) ↔ node(20) ↔ following(30)
삭제      10.next = 30 / 30.prev = 10
더미      head ↔ 실제 원소들 ↔ tail
```

**주의** init() 후 사용. tail 뒤 삽입·더미 삭제는 금지. 노드 검색 O(n), 삽입·삭제 O(1), 공간 O(n).

[[Hashing]] · [[Stack and Queue]]
