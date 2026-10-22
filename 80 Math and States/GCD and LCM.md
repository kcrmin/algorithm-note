---
type: template
aliases: ["Euclidean Algorithm", "GCD_LCM", "Math Utilities"]
category: number-theory
categories: [number-theory]
---

# GCD and LCM

```java
long gcd(long a, long b) {
    while (b != 0) {
        long remainder = a % b; // (a,b)와 (b,a%b)의 공약수는 같다.
        a = b;
        b = remainder;
    }
    return a;
}
long lcm(long a, long b) {
    if (a == 0 || b == 0) return 0;
    return a / gcd(a, b) * b; // 먼저 나눠 중간 곱셈의 넘침을 줄인다.
}
```

a,b≥0, gcd(0,0)=0 · LCM 결과는 long 범위 · 시간 O(log(max(a,b)+1)), 공간 O(1).

**분류** [[00 Maps/Number Theory|Number Theory]]
