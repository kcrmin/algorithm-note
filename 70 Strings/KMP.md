---
type: template
tags: ["Strings"]
aliases: ["Knuth-Morris-Pratt"]
---

# KMP

```java
// 빈 패턴은 제외. 반환은 0-index 시작 위치 목록. java.util.* 사용.
// prefixFunction은 Prefix Function 노트의 함수다.
// j는 현재까지 일치한 패턴 접두사의 길이다.
List<Integer> findAll(String text, String pattern) {
    int[] pi = prefixFunction(pattern);
    List<Integer> matches = new ArrayList<>();
    int m = pattern.length();
    for (int i = 0, j = 0; i < text.length(); i++) {
        while (j > 0 && text.charAt(i) != pattern.charAt(j)) {
            // 남은 접미사로 다음 후보 길이를 정한다.
            // 또 다를 수 있어 while. 텍스트 i는 되돌리지 않는다.
            j = pi[j - 1];
        }
        // 다음 한 글자도 맞으면 일치 길이를 늘린다.
        if (text.charAt(i) == pattern.charAt(j)) j++;
        if (j == m) {
            // i는 끝 위치이므로 길이 m만큼 거슬러 시작을 기록한다.
            matches.add(i - m + 1);
            // 겹친 일치를 남기고 다음 반복의 pattern[m] 접근도 막는다.
            j = pi[j - 1];
        }
    }
    return matches;
}
```

**예시**

```text
텍스트  a b a b a b a
패턴    a b a b a        → 시작 0 발견
            a b a b a    → 시작 2 발견
첫 일치 뒤 j: 5 → pi[4]=3
끝의 aba를 다음 패턴의 앞 aba로 남긴다.
```

**주의** `pi`는 텍스트가 아닌 패턴으로 만든다. O(N+M) 시간·O(M) 보조 공간+결과.

[[Prefix Function]] · [[String Hashing]] · [[Trie]]
