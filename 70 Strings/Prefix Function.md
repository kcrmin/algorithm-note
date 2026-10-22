---
type: template
tags: ["Strings"]
aliases: ["KMP Failure Function"]
---

# Prefix Function

```java
// pi[i]: pattern[0..i]의 접두사=접미사 최대 길이. 자기 자신 전체는 제외한다.
// j는 현재 겹치는 길이이며, pattern[j]는 다음에 맞춰 볼 문자다.
int[] prefixFunction(String pattern) {
    int[] pi = new int[pattern.length()];
    for (int i = 1, j = 0; i < pattern.length(); i++) {
        while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) {
            // 이미 맞은 부분의 접미사를 다음 접두사 후보로 쓴다.
            // 한 번 줄여도 다를 수 있어 while로 반복한다.
            j = pi[j - 1];
        }
        // 같은 글자가 붙으면 접두사·접미사 모두 한 칸 늘어난다.
        if (pattern.charAt(i) == pattern.charAt(j)) j++;
        pi[i] = j;
    }
    return pi;
}
```

**예시**

```text
문자       a b a b a c a
pi         0 0 1 2 3 0 1
앞:       [a b a] b a
뒤:        a b [a b a]  → pi[4]=3
c 불일치: j=3 → pi[2]=1 → pi[0]=0
```

**주의** j는 인덱스이자 **이미 맞은 길이**다. 전체 문자열과의 일치는 제외한다. O(M) 시간·공간.

[[KMP]] · [[String Hashing]]
