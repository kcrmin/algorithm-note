---
type: template
tags: [문자열]
---

# Trie

문자별 경로를 공유해 단어와 접두사를 찾는다.

> **기억:** 한 글자마다 자식으로 이동한다. 경로 존재는 접두사, 마지막 terminal=true여야 저장된 단어.

**전제:** 소문자 a~z. 길이 L의 삽입·검색 O(L), 공간은 전체 삽입 길이×자식 26칸에 비례.

## 템플릿

`Node`에는 `Node[26] children`과 `boolean terminal`, `root`는 빈 루트가 있다. 선언은 아래 전체 구현에 있다.

```java
void insert(String word) {
    Node node = root;
    for (int i = 0; i < word.length(); i++) {
        int index = word.charAt(i) - 'a';
        if (node.children[index] == null) node.children[index] = new Node();
        node = node.children[index];
    }
    node.terminal = true;
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

## 왜 이렇게 쓰나

- 삽입은 자식이 없을 때만 만든다. 같은 접두사를 가진 단어가 앞부분 노드를 공유한다.
- 경로만 확인하면 apple을 저장한 뒤 app도 단어라고 오인한다. 단어 검색에는 terminal이 필요하다.
- `walk(prefix) != null`이면 접두사가 있다. 빈 문자열은 루트에 도착한다.

**추적:** 5글자 `apple`만 삽입: 3글자 `app`의 경로는 존재하지만 단어 검색은 false; app도 삽입하면 true.

> [!info]- 더 자세한 설명
>
> > **핵심:** 루트에서 노드까지의 경로가 하나의 접두사다. 경로의 존재와 단어의 끝을 나타내는 `terminal`을 구분한다.
>
> ## 개념
>
> Trie는 문자열의 문자를 한 간선씩 따라가며 공통 접두사를 공유하는 트리다. 단어 전체 비교를 반복하지 않고 검색 문자열의 길이만큼 이동하므로, 사전에서 단어와 접두사를 자주 찾을 때 유용하다. 경로가 있다고 반드시 저장된 단어는 아니므로 끝 노드에 별도 표시를 둔다.
>
> ## 적용 조건
>
> - 아래 템플릿의 알파벳은 소문자 `a`~`z`이며, 그 밖의 문자는 예외로 거부한다.
> - 길이 `L`인 문자열의 삽입·단어 검색·접두사 검색은 각각 `O(L)`이다.
> - 노드 수는 최대 `1 + 삽입한 문자열 길이의 합`이다. 각 노드가 자식 참조 26개를 가지므로 `HashSet`보다 메모리를 많이 사용할 수 있다. 단어 전체의 포함 여부만 필요하면 [[해시]]도 비교한다.
>
> 먼저 `apple`을 삽입하면 `root → a → p → p → l → e` 경로를 만들고 `e`의 `terminal`만 `true`가 된다. `app`를 삽입하면 기존 경로를 재사용하고 두 번째 `p`의 `terminal`도 `true`로 바꾼다.
>
> | 연산 | 도착한 노드 | 판정 근거 | 결과 |
> |---|---|---|---|
> | `contains("ap")` | 첫 번째 `p` | 경로는 있지만 `terminal = false` | `false` |
> | `startsWith("ap")` | 첫 번째 `p` | 경로가 존재 | `true` |
> | `contains("app")` | 두 번째 `p` | `terminal = true` | `true` |
> | `contains("apple")` | `e` | `terminal = true` | `true` |
> | `startsWith("apt")` | 없음 | `t` 자식이 없음 | `false` |
>
> `node = node.children[index]`는 현재까지 읽은 접두사에 해당하는 노드로 이동하는 코드다. `insert`는 자식이 없을 때만 생성하므로 접두사가 같은 단어는 앞부분 노드를 공유한다. `contains`가 `terminal`을 검사해야 `apple`만 저장한 사전에서 `app`를 저장된 단어로 오인하지 않는다.
>
> ### 주의점
>
> - 중복 삽입은 같은 `terminal`을 다시 `true`로 만들 뿐이다. 단어의 개수나 중복 횟수가 필요하면 별도의 카운터가 필요하다.
> - `startsWith("")`는 빈 Trie에서도 `true`다. `contains("")`는 처음에는 `false`이고 `insert("")` 이후에는 `true`다. 빈 문자열의 끝은 루트에 표시된다.
> - 알파벳이 크거나 동적이면 자식 배열 대신 `Map<Character, Node>`가 적합할 수 있다. 위 코드는 고정된 소문자 알파벳용이다.

> [!example]- 준비 코드 · 전체 구현
>
> ```java
> import java.util.Objects;
>
> public final class Trie {
>     private static final class Node {
>         final Node[] children = new Node[26];
>         boolean terminal;
>     }
>
>     private final Node root = new Node();
>
>     public void insert(String word) {
>         validate(word);
>         Node node = root;
>         for (int i = 0; i < word.length(); i++) {
>             int index = word.charAt(i) - 'a';
>             if (node.children[index] == null) {
>                 node.children[index] = new Node();
>             }
>             node = node.children[index];
>         }
>         node.terminal = true;
>     }
>
>     public boolean contains(String word) {
>         Node node = findNode(word);
>         return node != null && node.terminal;
>     }
>
>     public boolean startsWith(String prefix) {
>         return findNode(prefix) != null;
>     }
>
>     private Node findNode(String value) {
>         validate(value);
>         Node node = root;
>         for (int i = 0; i < value.length(); i++) {
>             int index = value.charAt(i) - 'a';
>             if (node.children[index] == null) {
>                 return null;
>             }
>             node = node.children[index];
>         }
>         return node;
>     }
>
>     private static void validate(String value) {
>         Objects.requireNonNull(value, "value");
>         for (int i = 0; i < value.length(); i++) {
>             char ch = value.charAt(i);
>             if (ch < 'a' || ch > 'z') {
>                 throw new IllegalArgumentException("only a-z is supported");
>             }
>         }
>     }
> }
> ```
