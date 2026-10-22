---
type: template
aliases: ["Euclidean Algorithm", "GCD_LCM", "Math Utilities"]
category: number-theory
categories: [number-theory]
---

# GCD and LCM

```text
GCD: 나머지로 두 수를 줄이기
(48,18) → (18,12) → (12,6) → (6,0) ⇒ GCD 6

LCM: 중복된 공약수만 한 번 제거
48 × 18 ÷ 6 = 144
└ 먼저 48 ÷ 6, 그다음 × 18
```

| Step | Pseudocode |
| --- | --- |
| Reduce | // 두 수를 두 번째 수와 나머지로 바꾼다. 공약수는 그대로다. |
| Finish | // 나머지가 0이면 마지막으로 남은 수가 최대공약수다. |
| LCM | // 한 수를 최대공약수로 먼저 나눈 뒤 다른 수를 곱한다. |

## Java

```java
long gcd(long a, long b) {
    while (b != 0) {
        long remainder = a % b;
        a = b;
        b = remainder;
    }
    return a;
}

long lcm(long a, long b) {
    if (a == 0 || b == 0) {
        return 0;
    }
    return a / gcd(a, b) * b;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `remainder` 먼저 저장 | a를 b로 덮어쓰기 전에 원래 a%b를 구해야 한다. |
| 나누기 → 곱하기 | 최종 LCM이 long에 들어가도 a*b는 먼저 넘칠 수 있다. 나눗셈을 앞에 둔다. |
| 0 처리 | gcd(a,0)=a, gcd(0,0)=0. lcm은 0을 먼저 처리해 0으로 나누지 않는다. |
| 입력 / 결과 범위 | a,b≥0 전용. LCM의 최종 결과도 long 범위여야 한다. |

최대공약수·최소공배수 · 시간 O(log(max(a,b)+1)), 공간 O(1).

**분류** [[00 Maps/Number Theory|Number Theory]]
