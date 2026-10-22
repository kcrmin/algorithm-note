---
type: template
tags: ["Strings"]
aliases: ["KMP Failure Function"]
category: string-algorithms
categories: [string-algorithms]
---

# Prefix Function

```text
pattern  A B A B A C
pi       0 0 1 2 3 0

문자열   ABABA
접두사   ABA..
접미사   ..ABA
         ABA = ABA → 길이 3

다음 C가 불일치: 후보 길이 3 → 1 → 0
                   ABA → A → 없음
```

| Step | Pseudocode |
| --- | --- |
| Carry | // 직전 위치에서 맞은 접두사 길이를 다음 후보로 가져온다. |
| Fallback | // 다음 글자가 다르면, 후보 안의 더 짧은 접두사·접미사 길이로 줄인다. |
| Extend | // 다음 글자가 같으면 길이를 하나 늘린다. |
| Save | // 현재 위치까지의 접두사·접미사 최대 길이를 기록한다. |

## Java

```java
int[] prefixFunction(String pattern) {
    int[] pi = new int[pattern.length()];

    for (int i = 1, j = 0; i < pattern.length(); i++) {
        while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) {
            // 맞았던 부분의 끝은 j - 1
            j = pi[j - 1];
        }
        if (pattern.charAt(i) == pattern.charAt(j)) {
            j++;
        }
        pi[i] = j;
    }
    return pi;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `pi[i]`는 길이 | `pattern[0..i]`의 접두사와 접미사가 같은 최대 길이. 문자열 전체 자신은 제외한다. |
| `i = 1`부터 | 한 글자는 자기 자신을 제외하면 겹치는 길이가 0이다. `pi[0]`은 그대로 0. |
| `j - 1` | `j`글자가 맞았다면 마지막 인덱스는 `j - 1`. `pi[j]`는 다른 구간의 값이다. |
| 빈 문자열 | 반복문을 건너뛰고 빈 배열을 반환한다. |

입력 문자열의 각 위치에 대한 접두사·접미사 길이 반환 · 시간·공간 O(m).

**분류** [[00 Maps/String Algorithms|String Algorithms]]
