---
type: template
aliases: ["Fermat Inverse"]
category: number-theory
categories: [number-theory]
---

# Modular Inverse

```text
mod 7에서 3의 역원은 5
3 × 5 = 15 ≡ 1 (mod 7)

4 ÷ 3  →  4 × 5  →  20 mod 7 = 6
          └ 역원을 곱하기

소수 p:  a^(p-1) ≡ 1  →  a × a^(p-2) ≡ 1
                                 └ a의 역원
```

| Step | Pseudocode |
| --- | --- |
| Check | // 나머지 기준이 소수이고, 나눌 수가 그 소수의 배수가 아닌지 확인한다. |
| Inverse | // 빠른 거듭제곱으로 역원을 구한다. 지수는 소수에서 2를 뺀 값이다. |
| Divide | // 정수 나눗셈 대신 분자에 분모의 역원을 곱하고 나머지를 구한다. |

## Java

```java
long inversePrime(long a, long p) {
    long result = 1, exponent = p - 2;
    a %= p;

    while (exponent > 0) {
        if ((exponent & 1) != 0) {
            result = result * a % p;
        }
        a = a * a % p;
        exponent /= 2;
    }
    return result;
}

long dividePrime(long numerator, long denominator, long p) {
    return numerator % p * inversePrime(denominator, p) % p;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `p - 2`인 이유 | 소수 p에서 a^(p−1)≡1. a 하나를 남기면 나머지 a^(p−2)가 역원이다. |
| 분모가 0인 나머지 | denominator%p=0이면 역원이 없다. 함수가 직접 검사하지 않으므로 호출 전에 보장한다. |
| 소수가 아닐 때 | a^(p−2)를 쓰면 안 된다. 일반 mod의 역원은 gcd(a,mod)=1일 때만 존재한다. |
| `/`가 아닌 `*` | 정수 나눗셈은 나머지 연산에서의 나눗셈을 대신하지 못한다. 역원을 곱한 뒤 나머지를 구한다. |

소수 2≤p≤Integer.MAX_VALUE · 입력≥0 · 역원 대상%p≠0 · 시간 O(log p), 공간 O(1).

**분류** [[00 Maps/Number Theory|Number Theory]]

**사용 원리** [[80 Math and States/Binary Exponentiation|Binary Exponentiation]]
