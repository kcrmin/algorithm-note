---
type: template
tags: ["Strings"]
aliases: ["Prefix Tree"]
category: string-algorithms
categories: [string-algorithms]
---

# Trie

```text
저장: car, cat, dog        * = 단어 끝

root
 ├─ c ─ a ─┬─ r*
 │         └─ t*
 └─ d ─ o ─── g*

ca  → 경로 있음 / 단어 아님
cat → 경로 있음 / 단어 맞음
```

| Step | Pseudocode |
| --- | --- |
| Insert | // 글자를 하나씩 따라가며 없는 노드를 만들고 마지막에 단어 끝을 표시한다. |
| Walk | // 글자 경로를 따라간다. 중간에 노드가 없으면 실패한다. |
| Contains | // 경로가 존재하고 마지막 노드에 단어 끝 표시까지 있어야 참이다. |

## Java

```java
class Node {
    Node[] children = new Node[26];
    boolean terminal;
}

Node root = new Node();

void insert(String word) {
    Node node = root;
    for (int i = 0; i < word.length(); i++) {
        int index = word.charAt(i) - 'a';
        if (node.children[index] == null) {
            node.children[index] = new Node();
        }
        node = node.children[index];
    }
    node.terminal = true;
}

Node walk(String word) {
    Node node = root;
    for (int i = 0; i < word.length(); i++) {
        node = node.children[word.charAt(i) - 'a'];
        if (node == null) {
            return null;
        }
    }
    return node;
}

boolean contains(String word) {
    Node node = walk(word);
    return node != null && node.terminal;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 단어 / 접두사 | 단어 검색은 `contains(word)`. 접두사 검색은 `walk(prefix) != null`; 끝 표시를 요구하지 않는다. |
| `terminal`의 위치 | 문자 반복이 끝난 뒤 표시한다. 중간 노드에 표시하면 접두사도 단어로 잘못 인정한다. |
| 문자 범위 | `children[26]`은 소문자 a~z 전용. 다른 문자 집합이면 인덱스 변환과 저장 구조를 함께 바꾼다. |
| 중복 삽입 | 같은 경로와 끝 표시를 재사용한다. 개수까지 세려면 별도 횟수가 필요하다. |

길이 L 문자열의 삽입·검색 O(L) · 공간 O(총 삽입 길이×26).

**분류** [[00 Maps/String Algorithms|String Algorithms]]
