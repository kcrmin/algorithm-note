---
type: technique
tags:
  - two-pointers
language: java
status: in-review
source: []
---

# Two Pointers
> **한 줄 요약:** 두 포인터를 단조롭게 이동시켜 전체를 한 번만 스캔하는 기법

## Core Concept
* **When to use:**
  - 포인터가 한 방향으로만 이동해도 되는 문제
* **Key Point:**
  - 단조성(monotonicity)이 핵심
  - 패턴은 최소한만 유지

## Patterns
### Pattern A: 정렬 배열에서 양끝 수렴 (필수)
- 정렬된 배열에서 두 값의 관계(합/차/쌍)
```java
int l = 0;
int r = n - 1;

while (l < r) {
    long s = a[l] + a[r];
    if (s == T) break;

    if (s < T) l++;  // 더 큰 합 필요
    else r--;        // 더 작은 합 필요
}
```

### Pattern B: Palindrome 체크 (필수)
- 문자열/배열이 팰린드롬인지 확인 (양끝에서 안쪽으로)
```java
boolean isPalindrome(char[] s) {
    int l = 0;
    int r = s.length - 1;

    while (l < r) {
        if (s[l] != s[r]) return false;
        l++;
        r--;
    }
    return true;
}
```

## Pitfalls & Tips
- 포인터가 되돌아가면 two pointers가 아님
- 정렬 양끝 수렴은 "정렬"이 전제
- Palindrome 류는 비교 대상이 문자/원소인지, 전처리(소문자/필터)가 필요한지 확인
