---
type: template
aliases: ["Prime Sieve", "Primes and Modular Inverse"]
---

# Sieve of Eratosthenes

```java
// limit≥0. prime[x]=true면 x는 소수. java.util.Arrays 사용.
boolean[] sieve(int limit) {
    boolean[] prime = new boolean[limit + 1];
    Arrays.fill(prime, true);
    prime[0] = false; // 0과 1은 소수가 아니다.
    if (limit >= 1) prime[1] = false;
    // 합성수는 √limit 이하 소인수가 있다. 곱하기 전에 long 변환.
    for (int p = 2; (long) p * p <= limit; p++) {
        if (!prime[p]) continue;
        // p²보다 작은 배수는 더 작은 소인수가 이미 지웠다.
        for (long x = (long) p * p; x <= limit; x += p) {
            prime[(int) x] = false;
        }
    }
    return prime;
}
```

**예시**

```text
후보       2 3 4 5 6 7 8 9 10
2의 배수       ×   ×   ×    ×   (4부터)
3의 배수                 ×      (9부터)
남은 소수   2 3   5   7
```

**주의** 0·1은 소수가 아니다. limit 크기의 배열이 필요하다. O(limit log log limit) 시간·O(limit) 공간.

[[Modular Inverse]] · [[GCD and LCM]]
