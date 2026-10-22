---
type: template
tags: ["Strings"]
aliases: ["Prefix Tree"]
---

# Trie

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
            node.children[index] = new Node(); // ①
        }
        node = node.children[index];
    }
    node.terminal = true; // ②
}
Node walk(String word) {
    Node node = root;
    for (int i = 0; i < word.length(); i++) {
        node = node.children[word.charAt(i) - 'a'];
        if (node == null) return null;
    }
    return node;
}
boolean contains(String word) {
    Node node = walk(word);
    return node != null && node.terminal; // ③
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 없을 때만 만들기 | 같은 접두사의 경로를 공유해야 검색도 같은 노드로 도착한다. |
| ② 끝 노드에 표시 | 경로가 있다는 사실만으로 저장된 단어인지 알 수 없다. |
| ③ `terminal`도 검사 | apple을 넣은 뒤 app을 단어라고 오인하지 않게 한다. |

**예시**

```text
apple만 저장: root → a → p → p → l → e★
                            ↑ app 경로는 있음
★ = 단어의 끝               app 단어는 없음
app도 저장:   root → a → p → p★→ l → e★
                            이제 app도 단어
```

**적용** 여러 단어가 있는 사전에서 단어 또는 접두사의 존재를 반복해서 찾을 때.

**준비** 소문자 a~z, `Node`가 실제 자료구조다. 접두사 검색은 `walk(prefix) != null`.

**주의** 글자마다 자식 26칸이 생겨 메모리가 크다. 길이 L 삽입·검색 O(L), 공간 O(전체 삽입 길이×26).

[[Hashing]] · [[KMP]]
