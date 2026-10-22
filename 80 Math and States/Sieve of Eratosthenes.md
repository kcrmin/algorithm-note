---
type: template
aliases: ["Prime Sieve", "Primes and Modular Inverse"]
---

# Sieve of Eratosthenes

```java
boolean[] sieve(int limit) {
    boolean[] prime = new boolean[limit + 1];
    Arrays.fill(prime, true);
    prime[0] = false;
    if (limit >= 1) prime[1] = false;
    for (int p = 2; (long) p * p <= limit; p++) {
        if (!prime[p]) continue;
        for (long x = (long) p * p; x <= limit; x += p) { // ①
            prime[(int) x] = false;
        }
    }
    return prime;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① p²부터 지움 | 더 작은 p의 배수는 작은 소인수 단계에서 이미 지워졌다. |
| p² ≤ limit까지만 | 합성수는 반드시 √limit 이하의 소인수를 갖는다. |
| `(long) p * p` | int끼리 먼저 곱하면 형변환 전에 넘칠 수 있다. |

**예시**

```text
후보       2 3 4 5 6 7 8 9 10
2의 배수       ×   ×   ×    ×   (4부터)
3의 배수                 ×      (9부터)
남은 소수   2 3   5   7
```

**적용** 0부터 limit까지의 소수 여부를 한꺼번에 구한다.

**준비** `prime[x]`가 true면 x는 소수. limit ≥ 0.

**주의** 0·1은 소수가 아니다. limit 크기의 배열이 필요하다. O(limit log log limit) 시간·O(limit) 공간.

[[Modular Inverse]] · [[GCD and LCM]]
