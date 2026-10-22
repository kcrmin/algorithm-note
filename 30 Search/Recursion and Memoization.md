---
type: template
tags: ["Search"]
aliases: ["Memoization", "Top-down DP"]
---

# Recursion and Memoization

```java
// Fibonacci: n≥0, memo/computed는 길이 n+1로 새로 생성. MOD=1_000_000_007.
static long solve(int n) {
    if (n <= 1) return n; // 상태가 작아져 이 바닥에 닿아야 재귀가 끝남
    if (computed[n]) return memo[n]; // 같은 상태는 하위 호출을 다시 펼치지 않음
    memo[n] = (solve(n - 1) + solve(n - 2)) % MOD;
    computed[n] = true; // 정답 0도 가능하므로 값과 계산 여부를 따로 저장
    return memo[n];
}
```

**예시**

```text
f(4) = f(3) + f(2)
       f(3) = f(2) + f(1)
처음 f(2)=1 → 저장
다음 f(2)   → 호출을 펼치지 않고 1 반환 → f(4)=3
```

**주의** 큰 n은 호출 깊이 때문에 반복 DP가 안전하다. O(n) 시간·공간.

[[DP]] · [[Backtracking]]
