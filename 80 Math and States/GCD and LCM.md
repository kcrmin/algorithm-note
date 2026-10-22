---
type: template
aliases: ["Euclidean Algorithm", "GCD_LCM", "Math Utilities"]
---

# GCD and LCM

**언제** 두 정수의 공통 단위나 주기가 필요하면 GCD·LCM을 계산한다.

```text
GCD: (12,8) → (8,4) → (4,0) → 4
       a,b     b,a%b           ↑ 답
LCM: 12 ÷ 4 × 8 = 24
공약수 4가 두 번 곱해지지 않게 한 번 나눈다
```

**코드** 입력은 0 이상. 둘 다 0이면 GCD=0, 하나라도 0이면 LCM=0.

```java
long gcd(long a, long b) {
    while (b != 0) {
        long remainder = a % b;
        a = b;
        b = remainder;             // ①
    }
    return a;
}

long lcm(long a, long b) {
    if (a == 0 || b == 0) return 0;
    return a / gcd(a, b) * b;       // ②
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① (a,b) → (b,a%b) | a와 b의 공약수는 b와 나머지의 공약수와 같다. |
| b == 0이면 a가 답 | 0은 모든 수로 나누어지므로 남은 a가 최대공약수다. |
| ② 나누고 곱하기 | a*b부터 계산하는 것보다 중간값의 넘침을 줄인다. |

**주의** LCM의 최종 결과도 long 범위 안이어야 한다. O(log(max(a,b)+1)) 시간, O(1) 공간.

[[Binary Exponentiation]] · [[Sieve of Eratosthenes]] · [[Java Basics]]
