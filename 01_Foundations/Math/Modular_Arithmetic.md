---
type: concept
tags:
  - math
  - modular-arithmetic
language: java
status: in-review
source: []
---

# Modular Arithmetic (모듈러 연산)

> **한 줄 요약:** 나머지 연산에서 덧셈·뺄셈·곱셈은 그대로 적용, 나눗셈은 역원(페르마 소정리) 사용

## Core Concept

- **When to use:** 큰 수를 특정 수로 나눈 나머지 요구, 조합론 (nCr mod p)
- **Key Point:**
  - (a + b) % M, (a - b + M) % M, (a * b) % M 은 각각 나눈 뒤 연산해도 됨
  - **나눗셈**: 모듈러 역원. M이 소수면 a^(-1) = a^(M-2) (mod M) (페르마 소정리)
  - 음수 나머지: `(a % M + M) % M` 으로 양수 나머지로 통일

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Add | `(a + b) % M` | 덧셈 |
| Sub | `(a - b + M) % M` | 뺄셈 (음수 방지) |
| Mul | `(a * b) % M` | 곱셈 (long으로 중간 계산) |
| Inv | `pow(a, M-2, M)` | a의 역원 (M 소수) |
| Div | `a * inv(b) % M` | a/b (mod M) |

## Patterns

### Pattern A: 모듈러 곱셈 (overflow 방지)

```java
long mul(long a, long b, long M) {
    return (a % M) * (b % M) % M;
}
```

### Pattern B: 모듈러 거듭제곱 (Fast Power)

```java
long pow(long a, long b, long M) {
    long r = 1;
    a %= M;
    for (; b > 0; b >>= 1) {
        if ((b & 1) == 1) r = r * a % M;
        a = a * a % M;
    }
    return r;
}
```

### Pattern C: 역원 (M 소수일 때)

```java
long inv(long a, long M) {
    return pow(a, M - 2, M);  // a^(M-2) mod M
}
// nCr = n! * inv(r!) * inv((n-r)!) % M
```

## Pitfalls & Tips

- 곱셈 전에 각 인자 먼저 % M 하면 overflow 감소 (long 범위 내에서)
- M이 소수가 아니면 확장 유클리드로 역원 구해야 함
- 뺄셈 후 음수: `(x % M + M) % M`
