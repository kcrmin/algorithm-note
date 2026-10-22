---
type: template
aliases: ["Fermat Inverse"]
---

# Modular Inverse

**언제** 소수 p로 나눈 나머지에서 a로 나누려면, a의 역원을 곱한다. a % p ≠ 0이어야 한다.

```text
10 ÷ 2 mod 7 = 5          ← 구하려는 답
나머지끼리 3 ÷ 2 = 1      ← 정수 나눗셈은 틀림
2⁻¹ ≡ 2⁵ ≡ 4 (mod 7)     ← 2 × 4 % 7 = 1
나머지에 역원 곱함: 3 × 4 % 7 = 5
```

**코드** `modPow`는 [[Binary Exponentiation]]의 함수. a ≥ 0, 2 ≤ p ≤ Integer.MAX_VALUE.

```java
long inversePrime(long a, long p) {
    return modPow(a, p - 2, p);       // ①
}
long dividePrime(long numerator, long denominator, long p) {
    long inverse = inversePrime(denominator, p);
    return numerator % p * inverse % p; // ②
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 지수 p-2 | Fermat의 a^(p-1) ≡ 1에서 a 하나를 분리한 것이 역원이다. |
| ② 나눗셈을 곱셈으로 | 나머지를 구한 뒤 정수 나눗셈하면 원래 몫을 복구할 수 없다. |
| p는 소수, a%p ≠ 0 | 이 조건에서만 위 역원 공식이 보장된다. |

**주의** 합성수 mod에는 이 공식을 적용하지 않는다. numerator ≥ 0. O(log p) 시간, O(1) 공간.

[[Binary Exponentiation]] · [[Sieve of Eratosthenes]] · [[GCD and LCM]]
