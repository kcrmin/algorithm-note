---
type: template
aliases: ["Prime Sieve", "Primes and Modular Inverse"]
category: number-theory
categories: [number-theory]
---

# Sieve of Eratosthenes

```java
boolean[] sieve(int limit) {
    boolean[] prime = new boolean[limit + 1];
    // seed: 모두 후보로 두고 0·1 제외
    Arrays.fill(prime, true);
    prime[0] = false;
    if (limit >= 1) prime[1] = false;
    // scan: sqrt(limit) 이하 소수의 배수 제거
    for (int p = 2; (long) p * p <= limit; p++) {
        if (!prime[p]) continue;
        // mark: 작은 배수는 이미 지워져 p²부터
        for (long x = (long) p * p; x <= limit; x += p) prime[(int) x] = false;
    }
    return prime;
}
```

limit≥0, prime[x]는 소수 여부 · 시간 O(limit log log limit), 공간 O(limit).

**분류** [[00 Maps/Number Theory|Number Theory]]
