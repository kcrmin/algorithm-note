---
type: template
aliases: ["Fermat Inverse"]
category: number-theory
categories: [number-theory]
---

# Modular Inverse

```java
// inversePrime: 페르마 정리로 a^(p-2) 계산
long inversePrime(long a, long p) {
    long result = 1, exponent = p - 2;
    a %= p;
    while (exponent > 0) {
        // take: 켜진 비트의 거듭제곱 곱하기
        if ((exponent & 1) != 0) result = result * a % p;
        // advance: 밑 제곱·지수 절반으로 다음 비트 준비
        a = a * a % p;
        exponent /= 2;
    }
    return result;
}
// dividePrime: 정수 나눗셈 대신 역원 곱하기
long dividePrime(long numerator, long denominator, long p) {
    return numerator % p * inversePrime(denominator, p) % p;
}
```

소수 2≤p≤Integer.MAX_VALUE, 입력≥0, 역원 대상%p≠0 · 시간 O(log p), 공간 O(1).

**분류** [[00 Maps/Number Theory|Number Theory]]

**사용 원리** [[80 Math and States/Binary Exponentiation|Binary Exponentiation]]
