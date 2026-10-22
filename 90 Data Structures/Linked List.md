---
type: template
aliases: ["Doubly Linked List", "Linked_List", "Doubly_Linked_List"]
category: core-collections
categories: [core-collections]
---

# Linked List

```text
삽입 전:  head ⇄ A ⇄ B ⇄ tail
                    ↓ A 뒤에 X 삽입
삽입 후:  head ⇄ A ⇄ X ⇄ B ⇄ tail

X 삭제:   head ⇄ A ─────⇄ B ⇄ tail
                 이웃끼리 다시 연결

head / tail은 값으로 쓰지 않는 양끝 더미
```

| Step | Pseudocode |
| --- | --- |
| Init | // 양끝 더미를 서로 연결해 빈 리스트를 만든다. |
| Save | // 삽입 위치의 원래 오른쪽 이웃을 먼저 저장한다. |
| Insert | // 새 노드의 양쪽 이웃을 지정하고, 이웃들의 연결을 새 노드로 바꾼다. |
| Remove | // 삭제할 노드의 양쪽 이웃을 서로 연결한다. |

## Java

```java
class Node {
    int value;
    Node prev, next;

    Node(int value) {
        this.value = value;
    }
}

Node head = new Node(0), tail = new Node(0);

void init() {
    head.next = tail;
    tail.prev = head;
}

Node insertAfter(Node previous, int value) {
    // previous.next를 바꾸기 전에 원래 이웃을 보관한다.
    Node following = previous.next;
    Node node = new Node(value);

    node.prev = previous;
    node.next = following;

    previous.next = node;
    following.prev = node;
    return node;
}

void remove(Node node) {
    node.prev.next = node.next;
    node.next.prev = node.prev;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 연결 4개 | 삽입은 새 노드→양 이웃 2개, 양 이웃→새 노드 2개. 한쪽만 바꾸면 역방향 연결이 깨진다. |
| 원래 이웃 먼저 저장 | previous.next부터 바꾸면 기존 오른쪽 이웃을 잃을 수 있다. |
| 더미의 역할 | 맨 앞 삽입도 `insertAfter(head, value)`로 처리한다. tail 뒤 삽입과 더미 삭제는 금지한다. |
| 노드를 알아야 O(1) | 삽입할 이전 노드나 삭제할 노드가 이미 있어야 한다. 값으로 찾는 검색은 O(n). 삭제된 노드를 다시 삭제하지 않는다. |

`init()` 후 사용 · 삽입·삭제 O(1), 검색 O(n), 공간 O(n).

**분류** [[00 Maps/Core Collections|Core Collections]]
