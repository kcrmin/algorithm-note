---
type: template
aliases: ["Doubly Linked List", "Linked_List", "Doubly_Linked_List"]
category: core-collections
categories: [core-collections]
---

# Linked List

```java
class Node {
    int value;
    Node prev, next;
    Node(int value) { this.value = value; }
}
Node head = new Node(0), tail = new Node(0);
// init: 양끝 더미를 연결해 빈 리스트 준비
void init() { head.next = tail; tail.prev = head; }
// insertAfter: 이전 노드와 오른쪽 이웃 사이에 연결
Node insertAfter(Node previous, int value) {
    // save: 연결 변경 전 원래 오른쪽 이웃 보관
    Node following = previous.next;
    Node node = new Node(value);
    node.prev = previous;
    node.next = following;
    previous.next = node;
    following.prev = node;
    return node;
}
// remove: 양 이웃끼리 연결해 삭제 노드 건너뛰기
void remove(Node node) {
    node.prev.next = node.next;
    node.next.prev = node.prev;
}
```

init() 후 사용 · tail 뒤 삽입·더미 삭제 금지 · 노드를 알면 삽입·삭제 O(1), 검색 O(n), 공간 O(n).

**분류** [[00 Maps/Core Collections|Core Collections]]
