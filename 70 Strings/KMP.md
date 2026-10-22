---
type: template
tags: ["Strings"]
aliases: ["Knuth-Morris-Pratt"]
---

# KMP

```java
List<Integer> findAll(String text, String pattern) {
    int[] pi = prefixFunction(pattern);
    List<Integer> matches = new ArrayList<>();
    int m = pattern.length();
    for (int i = 0, j = 0; i < text.length(); i++) {
        while (j > 0 && text.charAt(i) != pattern.charAt(j)) {
            j = pi[j - 1]; // ①
        }
        if (text.charAt(i) == pattern.charAt(j)) j++;
        if (j == m) {
            matches.add(i - m + 1); // ②
            j = pi[j - 1]; // ③
        }
    }
    return matches;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① j만 되돌리기 | 이미 읽은 텍스트는 재검사할 필요 없이, 남은 접미사를 이용한다. |
| ② `i-m+1` | i는 일치의 끝이다. 길이 m만큼 거슬러 시작을 기록한다. |
| ③ 일치 후에도 후퇴 | j=0이면 겹친 일치를 놓친다. j=m인 채면 다음 글자에서 범위를 넘는다. |

**예시**

```text
텍스트  a b a b a b a
패턴    a b a b a        → 시작 0 발견
            a b a b a    → 시작 2 발견
첫 일치 뒤 j: 5 → pi[4]=3
끝의 aba를 다음 패턴의 앞 aba로 남긴다.
```

**적용** 한 패턴의 모든 등장 위치를 겹치는 일치까지 정확히 찾을 때. 패턴은 비어 있으면 안 된다.

**준비** `Prefix Function`의 `prefixFunction(pattern)`을 함께 둔다. 반환은 **0-index 시작 위치**, `java.util.*` 사용.

**주의** `pi`는 텍스트가 아닌 패턴으로 만든다. O(N+M) 시간·O(M) 보조 공간+결과.

[[Prefix Function]] · [[String Hashing]] · [[Trie]]
