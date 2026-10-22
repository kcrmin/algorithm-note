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
    Arrays.fill(prime, true);
    prime[0] = false;
    if (limit >= 1) prime[1] = false;
    for (int p = 2; (long) p * p <= limit; p++) {
        if (!prime[p]) continue;
        // p²보다 작은 배수는 더 작은 소인수가 이미 지웠다.
        for (long x = (long) p * p; x <= limit; x += p) prime[(int) x] = false;
    }
    return prime;
}
```

limit≥0, prime[x]는 소수 여부 · 시간 O(limit log log limit), 공간 O(limit).

**분류** [[00 Maps/Number Theory|Number Theory]]
