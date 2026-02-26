---
type: concept
tags:
  - math
  - gcd
  - lcm
language: java
status: in-review
source: []
---

# GCD & LCM (최대공약수, 최소공배수)

> **한 줄 요약:** 유클리드 호제법으로 GCD를 O(log min(a,b))에 구하고, LCM = a*b/GCD

## Core Concept

- **When to use:** 분수 약분, 주기/반복, 정수론 기초
- **Key Point:**
  - **GCD(a, b) = GCD(b, a % b)** (a ≥ b), b==0이면 a가 답
  - **LCM(a, b) = a * b / GCD(a, b)** (오버플로우 주의: a/GCD*b 형태로)
  - 여러 수의 GCD/LCM은 두 개씩 묶어서 반복

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| GCD | `gcd(a, b)` | 최대공약수 |
| LCM | `lcm(a, b) = a / gcd(a,b) * b` | 최소공배수 (overflow 방지) |
| Extended | `gcd(a,b)` 동시에 x,y 구하면 확장 유클리드 | ax + by = gcd(a,b) |

## Patterns

### Pattern A: GCD (유클리드 호제법)

```java
long gcd(long a, long b) {
    while (b != 0) {
        long t = b;
        b = a % b;
        a = t;
    }
    return a;
}
// 또는 재귀: return b == 0 ? a : gcd(b, a % b);
```

### Pattern B: LCM (overflow 방지)

```java
long lcm(long a, long b) {
    return a / gcd(a, b) * b;  // 먼저 나누기
}
```

### Pattern C: 여러 수의 GCD

```java
long g = a[0];
for (int i = 1; i < n; i++) g = gcd(g, a[i]);
```

## Pitfalls & Tips

- LCM 계산 시 `a * b / gcd`는 a*b에서 overflow → `a / gcd * b` 순서로
- 음수: gcd(|a|, |b|)로 통일해서 사용
- GCD(0, b) = b, GCD(a, 0) = a
