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
            j = pi[j - 1]; // 맞았던 접미사를 다음 접두사 후보로 줄인다.
        }
        if (pattern.charAt(i) == pattern.charAt(j)) j++;
        pi[i] = j;
    }
    return pi;
}
```

pi[i]: pattern[0..i]의 접두사=접미사 최대 길이, 전체 자신은 제외 · 시간·공간 O(m).

**분류** [[00 Maps/String Algorithms|String Algorithms]]
