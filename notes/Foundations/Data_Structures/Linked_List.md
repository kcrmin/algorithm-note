---
type: data-structure
tags:
  - data-structure
  - linked-list
language: java
status: in-review
source: []
---

# Linked List (단일 연결 리스트)

> **한 줄 요약:** 노드를 포인터(참조)로 이어 붙여 순차 자료를 표현하는 선형 자료구조

## Core Concept

- **When to use:**
  - 중간 삽입/삭제가 잦고, 임의 접근(random access)이 굳이 필요 없을 때
  - 배열처럼 한 번에 큰 연속 메모리를 잡기 어려울 때
- **Key Point:**
  - 각 노드는 `값 + 다음 노드(next) 참조`로 구성
  - 인덱스 기반 O(1) 접근은 불가, k번째 원소 접근은 O(N)
  - 하지만 **노드 참조를 알고 있다면** 그 앞뒤 삽입/삭제는 O(1)에 가능

## API & State Change

| Operation | Code Snippet | 설명 |
| :--- | :--- | :--- |
| Init | `Node head = null;` | 빈 리스트 |
| Push front | `addFirst(x)` | 맨 앞에 삽입 |
| Push back | `addLast(x)` | 맨 뒤에 삽입(테일 포인터 있으면 O(1)) |
| Remove front | `removeFirst()` | 맨 앞 삭제 |
| Find | `find(x)` | 값 x를 가진 노드 탐색 |

## Patterns

### Pattern A: 기본 단일 연결 리스트

```java
class Node {
    int val;
    Node next;

    Node(int val) {
        this.val = val;
    }
}

class SinglyLinkedList {
    Node head;
    Node tail; // 없으면 addLast가 O(N)

    void addFirst(int x) {
        Node node = new Node(x);
        node.next = head;
        head = node;
        if (tail == null) tail = head;
    }

    void addLast(int x) {
        Node node = new Node(x);
        if (head == null) {
            head = tail = node;
            return;
        }
        tail.next = node;
        tail = node;
    }

    void removeFirst() {
        if (head == null) return;
        head = head.next;
        if (head == null) tail = null;
    }

    Node find(int x) {
        for (Node cur = head; cur != null; cur = cur.next) {
            if (cur.val == x) return cur;
        }
        return null;
    }
}
```

### Pattern B: 중간 삽입/삭제 (노드 참조를 알고 있을 때)

- 단일 연결 리스트는 **이전 노드(prev)** 를 모르면 삭제가 불편
- 하지만 아래처럼 "뒤 노드를 복사" 하는 트릭이 가끔 문제로 출제됨

```java
// node: 삭제하고 싶은 노드 (마지막 노드가 아니라고 가정)
void deleteGivenNode(Node node) {
    if (node == null || node.next == null) return;
    node.val = node.next.val;
    node.next = node.next.next;
}
```

## Pitfalls & Tips

- 임의 접근이 필요한 상황(예: 자주 a[k]를 읽는 문제)에서는 배열/ArrayList가 보통 더 낫다
- 노드 삽입/삭제 시 **연결 끊김(next 갱신 누락)** 이 가장 흔한 버그
- GC 없는 언어(C/C++)라면 삭제 후 잘못된 포인터 사용(댕글링 포인터)에 주의
- 온라인 저지에서 성능 문제가 아니라면, **문제 풀이에서는 보통 Java의 `LinkedList` 보다는 배열/ArrayList를 선호** (메모리와 상수 시간 손해)
