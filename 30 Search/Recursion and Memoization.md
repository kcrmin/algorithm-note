---
type: template
tags: ["Search"]
aliases: ["Memoization", "Top-down DP"]
---

# Recursion and Memoization

**언제** 같은 상태의 답을 재귀에서 반복 계산할 때, 처음 계산한 답을 저장한다.

```text
f(4) = f(3) + f(2)
       f(3) = f(2) + f(1)
처음 f(2)=1 → 저장
다음 f(2)   → 호출을 펼치지 않고 1 반환 → f(4)=3
```

**코드** 예시는 Fibonacci. `long[] memo`, `boolean[] computed`는 길이 n+1, `MOD=1_000_000_007`.

```java
static long solve(int n) {
    if (n <= 1) return n; // ①
    if (computed[n]) return memo[n];
    memo[n] = (solve(n - 1) + solve(n - 2)) % MOD;
    computed[n] = true; // ②
    return memo[n];
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 종료 → 캐시 → 계산 | 작아지는 상태가 종료 조건에 도달해야 호출이 돌아온다. |
| ② computed는 별도 | 정답 0과 아직 계산하지 않은 상태를 구분한다. |
| 상태 = 캐시의 주소 | 답에 영향을 주는 값이 다르면 다른 칸에 저장해야 한다. |

**주의** 큰 n은 호출 깊이 때문에 반복 DP가 안전하다. O(n) 시간·공간.

[[DP]] · [[Backtracking]]
