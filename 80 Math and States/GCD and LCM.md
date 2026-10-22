---
type: template
aliases: ["Euclidean Algorithm", "GCD_LCM", "Math Utilities"]
category: number-theory
categories: [number-theory]
---

# GCD and LCM

```java
// gcd: 공약수가 같은 (b,a%b)로 줄이기
long gcd(long a, long b) {
    while (b != 0) {
        long remainder = a % b;
        a = b;
        b = remainder;
    }
    return a;
}
// lcm: gcd로 먼저 나눈 뒤 곱해 중간 넘침 줄이기
long lcm(long a, long b) {
    if (a == 0 || b == 0) return 0;
    return a / gcd(a, b) * b;
}
```

a,b≥0, gcd(0,0)=0 · LCM 결과는 long 범위 · 시간 O(log(max(a,b)+1)), 공간 O(1).

**분류** [[00 Maps/Number Theory|Number Theory]]
