---
type: template
aliases: ["Fermat Inverse"]
---

# Modular Inverse

```java
// Binary Exponentiation의 modPow 사용. p는 소수, 2≤p≤Integer.MAX_VALUE.
// 입력은 0 이상, denominator%p≠0. 0에는 역원이 없다.
long inversePrime(long a, long p) {
    // a^(p-1)≡1에서 a 하나를 분리하면 역원 a^(p-2)가 된다.
    return modPow(a, p - 2, p);
}
long dividePrime(long numerator, long denominator, long p) {
    long inverse = inversePrime(denominator, p);
    // 나머지끼리 정수 나눗셈하면 틀린다. 나눗셈 대신 역원을 곱한다.
    return numerator % p * inverse % p;
}
```

**예시**

```text
10 ÷ 2 mod 7 = 5          ← 구하려는 답
나머지끼리 3 ÷ 2 = 1      ← 정수 나눗셈은 틀림
2⁻¹ ≡ 2⁵ ≡ 4 (mod 7)     ← 2 × 4 % 7 = 1
나머지에 역원 곱함: 3 × 4 % 7 = 5
```

**주의** 합성수 mod에는 이 공식을 적용하지 않는다. numerator ≥ 0. O(log p) 시간, O(1) 공간.

[[Binary Exponentiation]] · [[Sieve of Eratosthenes]] · [[GCD and LCM]]
