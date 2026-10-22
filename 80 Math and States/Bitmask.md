---
type: template
aliases: ["Subset Enumeration"]
---

# Bitmask

**언제** 원소가 약 20개 이하일 때, 선택 여부를 비트로 나타내 모든 부분집합을 확인한다.

```text
인덱스 i      2  1  0
값 arr[i]     7  5  2
mask=5        1  0  1  → 7+2=9
1 << i        4  2  1  → 해당 자리만 켠다
```

**코드** 합이 target인 인덱스 부분집합의 개수. 빈 집합도 포함한다.

```java
long countSubsets(int[] arr, long target) {
    int n = arr.length;
    long answer = 0;
    for (int mask = 0; mask < (1 << n); mask++) {
        long sum = 0;
        for (int i = 0; i < n; i++) {
            if ((mask & (1 << i)) != 0) sum += arr[i]; // ①
        }
        if (sum == target) answer++;
    }
    return answer;
}
```

| 암기 포인트 | 이유 |
|---|---|
| `1 << n`개 | 각 원소마다 선택/제외 두 가지이므로 2ⁿ개다. |
| ① AND 결과 != 0 | 선택된 자리의 값은 1뿐 아니라 2·4·8일 수도 있다. |
| mask = 0 | 아무 원소도 고르지 않은 집합이며 합은 0이다. |

**주의** 같은 값도 인덱스가 다르면 다른 선택이다. n ≤ 20, O(n·2ⁿ) 시간, 추가 공간 O(1).

[[Backtracking]] · [[Brute Force]] · [[Game DP]]
