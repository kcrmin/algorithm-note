---
type: template
aliases: ["Fast Power", "Modular_Arithmetic"]
---

# Binary Exponentiation

```java
// a·exponent≥0, 1≤mod≤Integer.MAX_VALUE: 나머지끼리의 곱이 long 안에 들어온다.
long modPow(long a, long exponent, long mod) {
    long result = 1 % mod; // 지수 0도 처리한다. mod=1이면 1의 나머지도 0이다.
    a %= mod;
    while (exponent > 0) {
        // 홀수 지수의 남는 a 하나를 결과로 옮긴다.
        if ((exponent & 1) != 0) result = result * a % mod;
        a = a * a % mod; // a²을 새 밑으로 삼으면 남은 지수는 절반으로 줄어든다.
        exponent /= 2;
    }
    return result;
}
```

**예시**

```text
3⁵ mod 7,  5 = 101₂ = 4+1
a의 거듭제곱   3¹=3 → 3²=2 → 3⁴=4  (mod 7)
지수의 비트      1       0       1
선택한 것만 곱함: 3 × 4 mod 7 = 5
```

**주의** mod 상한은 long 곱셈 넘침을 막기 위한 조건이다. O(log(exponent+1)) 시간, O(1) 공간.

[[Modular Inverse]] · [[Bitmask]] · [[String Hashing]]
