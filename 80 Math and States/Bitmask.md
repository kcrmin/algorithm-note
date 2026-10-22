---
type: template
aliases: ["Subset Enumeration"]
---

# Bitmask

```java
// n≤20. 같은 값도 인덱스가 다르면 다른 선택이며, 빈 집합도 포함한다.
long countSubsets(int[] arr, long target) {
    int n = arr.length;
    long answer = 0;
    // 선택/제외 2ⁿ가지. mask=0은 아무것도 안 고른 집합.
    for (int mask = 0; mask < (1 << n); mask++) {
        long sum = 0; // 여러 int를 더한 합은 int 범위를 넘을 수 있다.
        for (int i = 0; i < n; i++) {
            // i번 비트만 검사. 결과는 2·4·8일 수도 있어 !=0.
            if ((mask & (1 << i)) != 0) sum += arr[i];
        }
        if (sum == target) answer++;
    }
    return answer;
}
```

**예시**

```text
인덱스 i      2  1  0
값 arr[i]     7  5  2
mask=5        1  0  1  → 7+2=9
1 << i        4  2  1  → 해당 자리만 켠다
```

**주의** 같은 값도 인덱스가 다르면 다른 선택이다. n ≤ 20, O(n·2ⁿ) 시간, 추가 공간 O(1).

[[Backtracking]] · [[Brute Force]] · [[Game DP]]
