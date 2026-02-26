---
type: algorithm
tags:
  - kmp
  - string
language: java
status: in-review
source: []
---

# KMP (Knuth-Morris-Pratt)

> **한 줄 요약:** 문자열 S에서 패턴 P가 나오는 모든 위치를 O(|S|+|P|)에 찾는 알고리즘

## Core Concept

- **When to use:** 문자열 매칭, 반복 패턴, 실패 함수(pi)로 접두사=접미사 최대 길이
- **Key Point:**
  - **실패 함수(pi)**: P[0..i]에서 접두사와 접미사가 일치하는 최대 길이 (자기 자신 제외)
  - 매칭 실패 시 pi를 따라 백트래킹해 다시 비교 (한 번에 많이 건너뜀)
  - pi 배열 구하는 과정도 P에 대해 같은 아이디어로 전처리

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Build pi | `pi[i]` = P[0..i] 접두사=접미사 최대 길이 | 전처리 |
| Match | S를 한 칸씩 진행, 불일치 시 pi로 j 이동 | 매칭 |

## Patterns

### Pattern A: 실패 함수(pi) 구하기

```java
int[] buildPi(String P) {
    int m = P.length();
    int[] pi = new int[m];
    for (int i = 1, j = 0; i < m; i++) {
        while (j > 0 && P.charAt(i) != P.charAt(j)) j = pi[j - 1];
        if (P.charAt(i) == P.charAt(j)) j++;
        pi[i] = j;
    }
    return pi;
}
```

### Pattern B: KMP 매칭 (위치 목록)

```java
List<Integer> match(String S, String P) {
    int[] pi = buildPi(P);
    int n = S.length(), m = P.length();
    List<Integer> pos = new ArrayList<>();
    for (int i = 0, j = 0; i < n; i++) {
        while (j > 0 && S.charAt(i) != P.charAt(j)) j = pi[j - 1];
        if (S.charAt(i) == P.charAt(j)) {
            j++;
            if (j == m) {
                pos.add(i - m + 1);
                j = pi[j - 1];
            }
        }
    }
    return pos;
}
```

## Pitfalls & Tips

- pi 인덱스: 0-based에서 pi[i]는 [0..i] 구간 기준
- j가 0일 때 더 이상 백트래킹하지 않음
- 반복 패턴 주기: n - pi[n-1] (n = P 길이, 나누어떨어지면 그 주기)
