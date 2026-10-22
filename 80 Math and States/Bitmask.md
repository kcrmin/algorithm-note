---
type: template
aliases: ["Subset Enumeration"]
category: exhaustive-search
categories: [exhaustive-search]
---

# Bitmask

```java
long countSubsets(int[] arr, long target) {
    int n = arr.length;
    long answer = 0;
    // enumerate: 빈 집합부터 2^n가지 선택 순회
    for (int mask = 0; mask < (1 << n); mask++) {
        long sum = 0;
        for (int i = 0; i < n; i++) {
            // select: 켜진 인덱스의 값만 합산
            if ((mask & (1 << i)) != 0) sum += arr[i];
        }
        // answer: 목표 합인 선택 집계
        if (sum == target) answer++;
    }
    return answer;
}
```

n≤20 · 인덱스가 다르면 별도 선택, 빈 집합 포함 · 시간 O(n·2ⁿ), 추가 공간 O(1).

**분류** [[00 Maps/Exhaustive Search|Exhaustive Search]]
