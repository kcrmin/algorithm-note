---
type: template
aliases: ["Fermat Inverse"]
category: number-theory
categories: [number-theory]
---

# Modular Inverse

```java
long inversePrime(long a, long p) {
    long result = 1, exponent = p - 2; // a^(p-1)≡1에서 역원은 a^(p-2)다.
    a %= p;
    while (exponent > 0) {
        if ((exponent & 1) != 0) result = result * a % p;
        a = a * a % p;
        exponent /= 2;
    }
    return result;
}
long dividePrime(long numerator, long denominator, long p) {
    return numerator % p * inversePrime(denominator, p) % p; // 나눗셈 대신 역원을 곱한다.
}
```

소수 2≤p≤Integer.MAX_VALUE, 입력≥0, 역원 대상%p≠0 · 시간 O(log p), 공간 O(1).

**분류** [[00 Maps/Number Theory|Number Theory]]

**사용 원리** [[80 Math and States/Binary Exponentiation|Binary Exponentiation]]
