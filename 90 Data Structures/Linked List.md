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
void init() { head.next = tail; tail.prev = head; }
Node insertAfter(Node previous, int value) {
    Node following = previous.next; // 연결을 바꾸기 전에 원래 오른쪽 이웃을 보관한다.
    Node node = new Node(value);
    node.prev = previous;
    node.next = following;
    previous.next = node;
    following.prev = node;
    return node;
}
void remove(Node node) {
    node.prev.next = node.next; // 양 이웃끼리 연결해 삭제 노드를 건너뛴다.
    node.next.prev = node.prev;
}
```

init() 후 사용 · tail 뒤 삽입·더미 삭제 금지 · 노드를 알면 삽입·삭제 O(1), 검색 O(n), 공간 O(n).

**분류** [[00 Maps/Core Collections|Core Collections]]
