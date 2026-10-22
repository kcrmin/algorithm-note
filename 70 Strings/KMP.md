---
type: template
tags: ["Strings"]
aliases: ["Knuth-Morris-Pratt"]
category: string-algorithms
categories: [string-algorithms]
---

# KMP

```java
List<Integer> findAll(String text, String pattern) {
    int m = pattern.length();
    int[] pi = new int[m];
    for (int i = 1, j = 0; i < m; i++) {
        // 맞았던 접미사를 다음 접두사 후보로 줄인다.
        while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) j = pi[j - 1];
        if (pattern.charAt(i) == pattern.charAt(j)) j++;
        pi[i] = j;
    }
    List<Integer> matches = new ArrayList<>();
    for (int i = 0, j = 0; i < text.length(); i++) {
        // 텍스트를 되돌리지 않고 남은 접미사만 재사용한다.
        while (j > 0 && text.charAt(i) != pattern.charAt(j)) j = pi[j - 1];
        if (text.charAt(i) == pattern.charAt(j)) j++;
        if (j == m) {
            matches.add(i - m + 1);
            j = pi[j - 1]; // 겹친 일치를 남기고 다음 pattern[m] 접근도 막는다.
        }
    }
    return matches;
}
```

빈 패턴 제외 · 0-index 시작 위치 반환 · 시간 O(n+m), 보조 공간 O(m)+결과.

**분류** [[00 Maps/String Algorithms|String Algorithms]]

**사용 원리** [[70 Strings/Prefix Function|Prefix Function]]

**비교** [[70 Strings/String Hashing|String Hashing]]
