---
type: template
aliases: ["Fast Power", "Modular_Arithmetic"]
category: number-theory
categories: [number-theory]
---

# Binary Exponentiation

```text
2¹³ = 2⁸ × 2⁴ × 2¹ = 8192

지수 13 = (1101)₂
비트        1    1    0    1
거듭제곱   2⁸   2⁴   2²   2¹
선택        ✓    ✓    ·    ✓

오른쪽부터: 홀수면 곱하기 → 밑 제곱 → 지수 절반
```

| Step | Pseudocode |
| --- | --- |
| Seed | // 곱셈의 시작값을 1로 두고 밑을 나머지 범위로 줄인다. |
| Take | // 지수의 마지막 비트가 켜져 있으면 현재 거듭제곱을 결과에 곱한다. |
| Advance | // 밑을 제곱하고 지수를 절반으로 줄여 다음 비트로 이동한다. |
| Return | // 지수를 모두 처리하면 선택한 거듭제곱의 곱이 답이다. |

## Java

```java
long modPow(long a, long exponent, long mod) {
    long result = 1 % mod;
    a %= mod;

    while (exponent > 0) {
        if ((exponent & 1) != 0) {
            result = result * a % mod;
        }

        a = a * a % mod;
        exponent /= 2;
    }
    return result;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 곱한 뒤 제곱 | 현재 비트에 해당하는 밑을 먼저 사용한다. 먼저 제곱하면 한 단계 뒤의 거듭제곱을 곱하게 된다. |
| `result = 1 % mod` | 지수 0은 1. 단, mod가 1이면 그 나머지는 0이다. |
| 음수 지수 / 밑 | 이 템플릿은 a≥0, exponent≥0. 음수 지수는 이 반복문으로 계산하지 않는다. |
| 곱셈 overflow | 1≤mod≤Integer.MAX_VALUE이면 두 나머지의 곱이 long 범위다. 더 큰 mod는 별도 곱셈 처리가 필요하다. |

반환: a^exponent mod mod · 시간 O(log(exponent+1)), 공간 O(1).

**분류** [[00 Maps/Number Theory|Number Theory]]
