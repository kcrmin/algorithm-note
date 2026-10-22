---
type: template
aliases: ["Fast Power", "Modular_Arithmetic"]
category: number-theory
categories: [number-theory]
---

# Binary Exponentiation

```java
long modPow(long a, long exponent, long mod) {
    // seed: 곱셈 항등원에서 시작, 지수 0도 처리
    long result = 1 % mod;
    a %= mod;
    while (exponent > 0) {
        // take: 켜진 비트의 거듭제곱을 답에 곱하기
        if ((exponent & 1) != 0) result = result * a % mod;
        // advance: 밑 제곱·지수 절반으로 다음 비트 준비
        a = a * a % mod;
        exponent /= 2;
    }
    return result;
}
```

a≥0, exponent≥0, 1≤mod≤Integer.MAX_VALUE로 long 곱셈 보장 · 시간 O(log(exponent+1)), 공간 O(1).

**분류** [[00 Maps/Number Theory|Number Theory]]
