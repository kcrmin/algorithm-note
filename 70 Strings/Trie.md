---
type: template
tags: ["Strings"]
aliases: ["Prefix Tree"]
---

# Trie

```java
// 소문자 a~z. 한 노드까지의 경로가 접두사이고 terminal은 저장한 단어의 끝이다.
// 접두사 존재는 walk(prefix)!=null, 단어 존재는 contains(word)로 구분한다.
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
            // 없을 때만 만들어 같은 접두사의 경로를 공유한다.
            node.children[index] = new Node();
        }
        node = node.children[index];
    }
    node.terminal = true; // 경로만으로는 단어의 끝인지 알 수 없어 별도로 표시한다.
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
    // apple만 넣었을 때 app을 저장된 단어로 오인하지 않는다.
    return node != null && node.terminal;
}
```

**예시**

```text
apple만 저장: root → a → p → p → l → e★
                            ↑ app 경로는 있음
★ = 단어의 끝               app 단어는 없음
app도 저장:   root → a → p → p★→ l → e★
                            이제 app도 단어
```

**주의** 글자마다 자식 26칸이 생겨 메모리가 크다. 길이 L 삽입·검색 O(L), 공간 O(전체 삽입 길이×26).

[[Hashing]] · [[KMP]]
