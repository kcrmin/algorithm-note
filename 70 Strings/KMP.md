---
type: template
tags: ["Strings"]
aliases: ["Knuth-Morris-Pratt"]
category: string-algorithms
categories: [string-algorithms]
---

# KMP

```text
text     A B A B A
match 1  A B A          → 시작 0
match 2      A B A      → 시작 2
             ↑
        이미 맞은 마지막 A를 재사용
        ABA → A : 다음 비교는 B부터
```

| Step | Pseudocode |
| --- | --- |
| Build | // 패턴 안에서 재사용할 접두사·접미사 길이를 구한다. |
| Fallback | // 불일치하면 이미 맞은 부분의 짧은 접두사로 돌아간다. 텍스트는 되돌리지 않는다. |
| Match | // 다음 글자가 같으면 일치 길이를 늘린다. |
| Record | // 패턴 전체가 맞으면 시작 위치를 저장하고, 겹친 부분을 남겨 계속 찾는다. |

## Java

```java
List<Integer> findAll(String text, String pattern) {
    int m = pattern.length();
    int[] pi = new int[m];

    for (int i = 1, j = 0; i < m; i++) {
        while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) {
            j = pi[j - 1];
        }
        if (pattern.charAt(i) == pattern.charAt(j)) {
            j++;
        }
        pi[i] = j;
    }

    List<Integer> matches = new ArrayList<>();
    for (int i = 0, j = 0; i < text.length(); i++) {
        // j = 지금까지 맞은 길이 = 다음에 비교할 패턴 인덱스
        while (j > 0 && text.charAt(i) != pattern.charAt(j)) {
            j = pi[j - 1];
        }
        if (text.charAt(i) == pattern.charAt(j)) {
            j++;
        }

        if (j == m) {
            matches.add(i - m + 1);
            // 0으로 지우면 겹쳐 있는 다음 일치를 놓친다.
            j = pi[j - 1];
        }
    }
    return matches;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `j = pi[j - 1]` | 맞은 부분의 끝은 `j - 1`. 그 부분의 접두사·접미사를 재사용한다. `j--`로 전부 다시 시도하지 않는다. |
| `while` → `if` | 한 번 줄여도 여전히 다를 수 있다. 맞거나 길이가 0이 될 때까지 줄인 뒤 확장한다. |
| `i - m + 1` | 현재 `i`는 일치한 구간의 마지막 위치다. 길이만큼 돌아가 시작 위치를 구한다. |
| 빈 패턴 | 이 템플릿은 `pattern.length() > 0`. 빈 패턴은 문제의 정의에 따라 별도 처리한다. |

빈 패턴 제외 · 모든 일치의 0-index 시작 위치 반환 · `java.util.*` · 시간 O(n+m), 보조 공간 O(m)+결과.

**분류** [[00 Maps/String Algorithms|String Algorithms]]

**사용 원리** [[70 Strings/Prefix Function|Prefix Function]]

**비교** [[70 Strings/String Hashing|String Hashing]]
