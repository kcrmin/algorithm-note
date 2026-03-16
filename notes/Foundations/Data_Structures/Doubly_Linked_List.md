---
type: data-structure
tags:
  - data-structure
  - doubly-linked-list
language: java
status: in-review
source: []
---

# Doubly Linked List (이중 연결 리스트)

> **한 줄 요약:** 앞/뒤 양방향 포인터로 양끝과 중간에서 삽입·삭제가 쉬운 리스트

## Core Concept

- **When to use:**
  - 양방향 순회가 필요할 때 (앞/뒤로 모두 이동)
  - 중간 노드를 알고 있을 때 그 앞/뒤에 자주 삽입/삭제할 때
- **Key Point:**
  - 각 노드는 `값 + prev + next` 세 가지 필드를 가짐
  - 단일 리스트보다 메모리를 더 쓰지만, 삭제/삽입 시 이전 노드를 쉽게 찾을 수 있음
  - 양 끝에서 삽입/삭제는 Deque와 비슷한 느낌 (head/tail 관리)

## API & State Change

| Operation | Code Snippet | 설명 |
| :--- | :--- | :--- |
| Init | `Node head = null, tail = null;` | 빈 리스트 |
| Push front | `addFirst(x)` | 맨 앞 삽입 |
| Push back | `addLast(x)` | 맨 뒤 삽입 |
| Remove node | `remove(node)` | 임의 노드 삭제 (노드 참조 필요, O(1)) |
| Insert before | `insertBefore(node, x)` | 특정 노드 앞에 삽입 |

## Patterns

### Pattern A: 기본 이중 연결 리스트 (직접 head/tail + 커서 관리)

```java
class DNode {
    int val;
    DNode prev, next;

    DNode(int val) {
        this.val = val;
    }
}

class DoublyLinkedList {
    DNode head, tail;
    // 중간 삽입/삭제를 자주 한다면, 이렇게 '커서' 노드를 하나 들고 다니는 패턴이 유용하다.
    DNode cursor;

    void addFirst(int x) {
        DNode node = new DNode(x);
        node.next = head;
        if (head != null) head.prev = node;
        head = node;
        if (tail == null) tail = node;
    }

    void addLast(int x) {
        DNode node = new DNode(x);
        node.prev = tail;
        if (tail != null) tail.next = node;
        tail = node;
        if (head == null) head = node;
    }

    void remove(DNode node) {
        if (node == null) return;
        if (node.prev != null) node.prev.next = node.next;
        else head = node.next;          // node가 head일 때
        if (node.next != null) node.next.prev = node.prev;
        else tail = node.prev;          // node가 tail일 때
    }

    // cursor 앞/뒤 삽입 예시
    void insertAfterCursor(int x) {
        if (cursor == null) return;
        DNode node = new DNode(x);
        node.prev = cursor;
        node.next = cursor.next;
        if (cursor.next != null) cursor.next.prev = node;
        else tail = node;
        cursor.next = node;
    }

    void insertBeforeCursor(int x) {
        if (cursor == null) return;
        DNode node = new DNode(x);
        node.next = cursor;
        node.prev = cursor.prev;
        if (cursor.prev != null) cursor.prev.next = node;
        else head = node;
        cursor.prev = node;
    }
}
```


### Pattern B: 더미(sentinel) head / tail 사용

- 항상 `head <-> ... <-> tail` 구조를 유지해서 **빈 리스트 / 한 개짜리 리스트 경계 처리가 단순해지는** 패턴
- 실제 데이터는 `head.next` 부터 `tail.prev` 사이에만 존재

```java
class DList {
    static class Node {
        int val;
        Node prev, next;
        Node(int v) { val = v; }
    }

    Node head, tail; // 더미 노드

    DList() {
        head = new Node(0);
        tail = new Node(0);
        head.next = tail;
        tail.prev = head;
    }

    // head 바로 뒤에 삽입
    void addFirst(int x) {
        insertAfter(head, new Node(x));
    }

    // tail 바로 앞에 삽입
    void addLast(int x) {
        insertBefore(tail, new Node(x));
    }

    void insertAfter(Node p, Node node) {
        node.prev = p;
        node.next = p.next;
        p.next.prev = node;
        p.next = node;
    }

    void insertBefore(Node p, Node node) {
        insertAfter(p.prev, node);
    }

    void remove(Node node) {
        if (node == head || node == tail) return; // 더미는 삭제 X
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    boolean isEmpty() {
        return head.next == tail;
    }
}
```

## Pitfalls & Tips

- `prev`/`next`를 동시에 갱신해야 해서 **한쪽만 바꾸고 다른 쪽을 빼먹는 버그**가 자주 생김
- head/tail 경계 처리(공집합, 원소 1개일 때) 로직을 항상 같이 체크
- 단일 리스트보다 메모리 사용량이 크므로, 정말 양방향이 필요한지 먼저 고민
- Java에서 단순 큐/덱 용도라면 직접 구현보다 `ArrayDeque`가 실전에서는 더 빠르고 간단한 경우가 많음
