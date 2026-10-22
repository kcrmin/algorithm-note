---
type: template
aliases: ["Fast Power", "Modular_Arithmetic"]
---

# Binary Exponentiation

```java
long modPow(long a, long exponent, long mod) {
    long result = 1 % mod;
    a %= mod;
    while (exponent > 0) {
        if ((exponent & 1) != 0) result = result * a % mod; // ①
        a = a * a % mod;                                  // ②
        exponent /= 2;
    }
    return result;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 홀수면 한 번 곱함 | 남는 a 한 개를 결과에 옮기면 지수를 짝수로 만들 수 있다. |
| ② 제곱하고 지수 절반 | a²을 새 밑으로 삼으면 남은 곱셈 횟수가 반으로 줄어든다. |
| `1 % mod`로 시작 | 지수 0의 답도 mod 범위에 맞춘다. mod=1이면 답은 0이다. |

**예시**

```text
3⁵ mod 7,  5 = 101₂ = 4+1
a의 거듭제곱   3¹=3 → 3²=2 → 3⁴=4  (mod 7)
지수의 비트      1       0       1
선택한 것만 곱함: 3 × 4 mod 7 = 5
```

**적용** aᵉ mod m을 계산할 때, 지수를 절반씩 줄여 큰 지수도 빠르게 처리한다.

**준비** a·exponent ≥ 0, 1 ≤ mod ≤ Integer.MAX_VALUE.

**주의** mod 상한은 long 곱셈 넘침을 막기 위한 조건이다. O(log(exponent+1)) 시간, O(1) 공간.

[[Modular Inverse]] · [[Bitmask]] · [[String Hashing]]
