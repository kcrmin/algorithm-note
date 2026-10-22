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
    // build: 패턴의 접두사=접미사 최대 길이 전처리
    for (int i = 1, j = 0; i < m; i++) {
        while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) j = pi[j - 1];
        if (pattern.charAt(i) == pattern.charAt(j)) j++;
        pi[i] = j;
    }
    List<Integer> matches = new ArrayList<>();
    for (int i = 0, j = 0; i < text.length(); i++) {
        // fallback: 텍스트는 고정, 패턴 후보만 이동
        while (j > 0 && text.charAt(i) != pattern.charAt(j)) j = pi[j - 1];
        // match: 다음 글자 일치 시 길이 확장
        if (text.charAt(i) == pattern.charAt(j)) j++;
        if (j == m) {
            // answer: m글자 일치의 시작 위치 저장
            matches.add(i - m + 1);
            // resume: 겹친 일치를 남겨 검색 계속
            j = pi[j - 1];
        }
    }
    return matches;
}
```

빈 패턴 제외 · 0-index 시작 위치 반환 · 시간 O(n+m), 보조 공간 O(m)+결과.

**분류** [[00 Maps/String Algorithms|String Algorithms]]

**사용 원리** [[70 Strings/Prefix Function|Prefix Function]]

**비교** [[70 Strings/String Hashing|String Hashing]]
