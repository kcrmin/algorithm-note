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
    for (int mask = 0; mask < (1 << n); mask++) { // 0은 빈 집합이다.
        long sum = 0;
        for (int i = 0; i < n; i++) {
            // 켜진 비트의 값은 2·4·8일 수도 있어 1이 아닌 0과 비교한다.
            if ((mask & (1 << i)) != 0) sum += arr[i];
        }
        if (sum == target) answer++;
    }
    return answer;
}
```

n≤20 · 인덱스가 다르면 별도 선택, 빈 집합 포함 · 시간 O(n·2ⁿ), 추가 공간 O(1).

**분류** [[00 Maps/Exhaustive Search|Exhaustive Search]]
