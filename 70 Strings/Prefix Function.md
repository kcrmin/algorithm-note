---
type: template
tags: ["Strings"]
aliases: ["KMP Failure Function"]
category: string-algorithms
categories: [string-algorithms]
---

# Prefix Function

```java
int[] prefixFunction(String pattern) {
    int[] pi = new int[pattern.length()];
    for (int i = 1, j = 0; i < pattern.length(); i++) {
        while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) {
            // fallback: 맞았던 접미사로 후보 길이 축소
            j = pi[j - 1];
        }
        // extend: 다음 글자까지 맞으면 한 칸 연장
        if (pattern.charAt(i) == pattern.charAt(j)) j++;
        // save: 현재 접두사=접미사 최대 길이
        pi[i] = j;
    }
    return pi;
}
```

pi[i]: pattern[0..i]의 접두사=접미사 최대 길이, 전체 자신은 제외 · 시간·공간 O(m).

**분류** [[00 Maps/String Algorithms|String Algorithms]]
