---
type: template
aliases: ["Fast Power", "Modular_Arithmetic"]
category: number-theory
categories: [number-theory]
---

# Binary Exponentiation

```java
long modPow(long a, long exponent, long mod) {
    long result = 1 % mod;
    a %= mod;
    while (exponent > 0) {
        if ((exponent & 1) != 0) result = result * a % mod; // 홀수의 남는 a 하나를 꺼낸다.
        a = a * a % mod; // 밑을 제곱하면 남은 지수를 절반으로 줄일 수 있다.
        exponent /= 2;
    }
    return result;
}
```

a≥0, exponent≥0, 1≤mod≤Integer.MAX_VALUE로 long 곱셈 보장 · 시간 O(log(exponent+1)), 공간 O(1).

**분류** [[00 Maps/Number Theory|Number Theory]]
