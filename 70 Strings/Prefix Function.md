---
type: template
tags: ["Strings"]
aliases: ["KMP Failure Function"]
---

# Prefix Function

**언제** 문자열의 앞부분과 뒷부분이 얼마나 겹치는지 구할 때. KMP의 불일치 후 이동 위치가 된다.

```text
문자       a b a b a c a
pi         0 0 1 2 3 0 1
앞:       [a b a] b a
뒤:        a b [a b a]  → pi[4]=3
c 불일치: j=3 → pi[2]=1 → pi[0]=0
```

**코드** `pi[i]`는 `pattern[0..i]`의 접두사=접미사 최대 길이. 자기 자신 전체는 제외한다.

```java
int[] prefixFunction(String pattern) {
    int[] pi = new int[pattern.length()];
    for (int i = 1, j = 0; i < pattern.length(); i++) {
        while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) {
            j = pi[j - 1]; // ①
        }
        if (pattern.charAt(i) == pattern.charAt(j)) j++; // ②
        pi[i] = j;
    }
    return pi;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① `j = pi[j-1]` | 맞았던 부분의 접미사여야 다음 접두사 후보가 될 수 있다. |
| `while`, 한 번이 아님 | 후보를 줄여도 새 문자가 또 다를 수 있다. |
| ② 같으면 `j++` | 겹치던 접두사 뒤에 한 글자를 더 붙일 수 있다. |

**주의** j는 인덱스이자 **이미 맞은 길이**다. 전체 문자열과의 일치는 제외한다. O(M) 시간·공간.

[[KMP]] · [[String Hashing]]
