---
type: template
tags: [문자열]
---

# KMP

> **핵심:** `j`는 지금까지 읽은 텍스트의 접미사와 일치하는 패턴 접두사의 길이다. 불일치하면 `pi[j - 1]`로 줄여 이미 확인한 문자를 다시 비교하지 않는다.

## 개념

KMP는 텍스트에서 패턴이 나타나는 모든 시작 위치를 선형 시간에 찾는다. 패턴의 접두사와 접미사가 얼마나 겹치는지 `pi` 배열에 저장해, 불일치하더라도 텍스트 위치를 뒤로 돌리지 않는다. 일치를 찾은 뒤에도 겹치는 부분을 남기므로 중첩된 등장까지 찾을 수 있다.

## 적용 조건

- 한 패턴을 긴 텍스트에서 찾거나, 겹치는 등장 횟수와 위치가 필요할 때 사용한다.
- 텍스트 길이 `N`, 패턴 길이 `M`에 대해 시간 `O(N + M)`, 보조 공간 `O(M)`이다. 반환하는 일치 위치 목록에는 추가로 `O(일치 개수)`가 필요하다.
- 아래 코드는 0-based 시작 위치를 반환한다. 빈 패턴은 `IllegalArgumentException`으로 거부하며, 빈 텍스트와 비어 있지 않은 패턴의 결과는 빈 목록이다.

## Java 템플릿

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Objects;

public final class KMP {
    // pi[i]: pattern[0..i]의 접두사이면서 접미사인
    // 가장 긴 진부분 문자열의 길이. 자기 자신 전체는 제외한다.
    public static int[] prefixFunction(String pattern) {
        Objects.requireNonNull(pattern, "pattern");
        int[] pi = new int[pattern.length()];

        for (int i = 1; i < pattern.length(); i++) {
            int j = pi[i - 1];
            while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) {
                j = pi[j - 1];
            }
            if (pattern.charAt(i) == pattern.charAt(j)) {
                j++;
            }
            pi[i] = j;
        }
        return pi;
    }

    public static List<Integer> findAll(String text, String pattern) {
        Objects.requireNonNull(text, "text");
        Objects.requireNonNull(pattern, "pattern");
        if (pattern.isEmpty()) {
            throw new IllegalArgumentException("pattern must not be empty");
        }

        int[] pi = prefixFunction(pattern);
        List<Integer> matches = new ArrayList<>();
        int j = 0;

        for (int i = 0; i < text.length(); i++) {
            while (j > 0 && text.charAt(i) != pattern.charAt(j)) {
                j = pi[j - 1];
            }
            if (text.charAt(i) == pattern.charAt(j)) {
                j++;
            }
            if (j == pattern.length()) {
                matches.add(i - pattern.length() + 1);
                j = pi[j - 1];
            }
        }
        return matches;
    }
}
```

> [!info]- 원리와 코드 설명
>
> `pi[i]`는 `pattern[0..i]` 끝부분에 남아 있는 접두사의 길이다. 예를 들어 패턴이 `ababaca`이면 다음과 같다.
>
> | 인덱스 `i` | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
> |---|---|---|---|---|---|---|---|
> | 문자 | a | b | a | b | a | c | a |
> | `pi[i]` | 0 | 0 | 1 | 2 | 3 | 0 | 1 |
>
> `i = 5`에서 `c`를 비교할 때 `j = 3`부터 시작한다. `pattern[3] = b`와 다르므로 `j = pi[2] = 1`, 다시 `pattern[1] = b`와 다르므로 `j = pi[0] = 0`으로 이동한다. 다음으로 가능한 접두사는 현재 접두사의 접미사여야 하므로 이 연결만 따라가면 된다. 한 번만 줄이는 `if`로는 여러 단계의 불일치를 처리할 수 없어 `while`이 필요하다.
>
> 검색에서 `text = abababa`, `pattern = ababa`이면 `pi = [0, 0, 1, 2, 3]`이다.
>
> | 처리 시점 | `j` 변화 | 결과 |
> |---|---|---|
> | 텍스트 인덱스 0~4 | 0 → 5 | 시작 위치 0을 기록 |
> | 첫 일치 직후 | 5 → `pi[4] = 3` | 끝의 `aba`를 다음 일치의 시작으로 보존 |
> | 텍스트 인덱스 5~6 | 3 → 5 | 시작 위치 2를 기록 |
>
> 완전 일치 뒤에 `j = 0`으로 초기화하면 위치 2의 겹치는 일치를 놓친다. `j`는 문자 하나를 읽을 때 최대 한 번 증가하며, 후퇴한 길이는 앞서 증가한 길이를 넘지 않으므로 중첩 `while`이 있어도 전체 시간은 선형이다.
>
> ### 주의점
>
> - `pi`는 텍스트가 아닌 **패턴**으로 만든다. 일치한 위치는 끝 인덱스 `i`가 아니라 `i - M + 1`이다.
> - 완전 일치 뒤 `j`를 반드시 줄여야 다음 반복에서 `pattern.charAt(M)`에 접근하지 않는다.
> - `charAt`과 인덱스는 UTF-16 코드 단위 기준이다. 대소문자 구분 등 비교 규칙을 바꾸려면 입력과 패턴에 같은 처리를 적용한다.
> - 관련: [[문자열 해싱]] — 반복 부분 문자열 비교, [[Trie]] — 여러 단어의 접두사 탐색.
