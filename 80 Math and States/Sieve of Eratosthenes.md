---
type: template
aliases: ["Prime Sieve", "Primes and Modular Inverse"]
category: number-theory
categories: [number-theory]
---

# Sieve of Eratosthenes

```text
범위: 2 ~ 12

p=2 → [4, 6, 8, 10, 12] 제거
p=3 → [9, 12] 제거        (12는 이미 제거됨)

남음: 2, 3, 5, 7, 11 → 소수

p=3:  3×2는 이미 2가 지움 → 3×3부터 제거
```

| Step | Pseudocode |
| --- | --- |
| Seed | // 모든 수를 후보로 두되 0과 1을 제외한다. |
| Scan | // 아직 남은 수만 소수로 보고 그 배수를 제거한다. |
| Mark | // 더 작은 배수는 이미 지워졌으므로 자기 자신을 제곱한 수부터 시작한다. |
| Finish | // 제곱이 범위를 넘으면 종료한다. 남은 후보가 소수다. |

## Java

```java
boolean[] sieve(int limit) {
    boolean[] prime = new boolean[limit + 1];
    Arrays.fill(prime, true);
    prime[0] = false;
    if (limit >= 1) {
        prime[1] = false;
    }

    for (int p = 2; (long) p * p <= limit; p++) {
        if (!prime[p]) {
            continue;
        }
        for (long x = (long) p * p; x <= limit; x += p) {
            prime[(int) x] = false;
        }
    }
    return prime;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 왜 √limit까지? | 합성수의 두 인수가 모두 √limit보다 클 수는 없다. 작은 인수 쪽에서 이미 제거된다. |
| `p * p`부터 | 2p, 3p, …의 작은 배수는 앞선 소수가 처리했다. p 자체는 소수이므로 지우지 않는다. |
| 곱하기 전 `(long)` | int끼리 먼저 곱한 뒤 변환하면 overflow를 되돌릴 수 없다. |
| 0과 1 | 둘 다 소수가 아니다. limit=0이면 인덱스 1이 없으므로 조건을 확인한다. |

limit≥0 · prime[x]는 x의 소수 여부 · `java.util.Arrays` · 시간 O(limit log log limit), 공간 O(limit).

**분류** [[00 Maps/Number Theory|Number Theory]]
