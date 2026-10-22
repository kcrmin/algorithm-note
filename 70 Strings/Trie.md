---
type: template
tags: ["Strings"]
aliases: ["Prefix Tree"]
category: string-algorithms
categories: [string-algorithms]
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
        if (node.children[index] == null) node.children[index] = new Node();
        node = node.children[index];
    }
    node.terminal = true; // 경로가 있어도 단어의 끝인지는 별도 표시가 필요하다.
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
    return node != null && node.terminal;
}
```

소문자 a~z · 접두사 존재는 walk(prefix)!=null · 길이 L 삽입·검색 O(L), 공간 O(총 삽입 길이×26).

**분류** [[00 Maps/String Algorithms|String Algorithms]]
