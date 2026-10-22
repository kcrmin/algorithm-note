---
type: template
aliases: ["Euclidean Algorithm", "GCD_LCM", "Math Utilities"]
---

# GCD and LCM

```java
// a,b≥0. LCM의 최종 결과는 long 범위여야 한다.
long gcd(long a, long b) {
    while (b != 0) {
        // (a,b)와 (b,a%b)의 공약수는 같고, 나머지는 더 작다.
        long remainder = a % b;
        a = b;
        b = remainder;
    }
    return a; // b가 0이면 남은 a가 최대공약수. gcd(0,0)=0으로 둔다.
}

long lcm(long a, long b) {
    if (a == 0 || b == 0) return 0; // 0이 있으면 LCM=0, 나눗셈도 건너뛴다.
    // 공통 인수를 한 번 나누고 곱해 중간 곱셈의 넘침을 줄인다.
    return a / gcd(a, b) * b;
}
```

**예시**

```text
GCD: (12,8) → (8,4) → (4,0) → 4
       a,b     b,a%b           ↑ 답
LCM: 12 ÷ 4 × 8 = 24
공약수 4가 두 번 곱해지지 않게 한 번 나눈다
```

**주의** LCM의 최종 결과도 long 범위 안이어야 한다. O(log(max(a,b)+1)) 시간, O(1) 공간.

[[Binary Exponentiation]] · [[Sieve of Eratosthenes]] · [[Java Basics]]
