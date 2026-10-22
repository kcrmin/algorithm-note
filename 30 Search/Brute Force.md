---
type: template
tags: ["Search"]
aliases: ["Exhaustive Search"]
category: exhaustive-search
categories: [exhaustive-search]
---

# Brute Force

```java
static long countPairs(int[] numbers, long target) {
    long count = 0;
    for (int i = 0; i < numbers.length; i++) {
        for (int j = i + 1; j < numbers.length; j++) { // i<j로 자기 자신·역순 쌍 제외
            if ((long) numbers[i] + numbers[j] == target) count++; // 더하기 전에 long으로 변환
        }
    }
    return count;
}
```

서로 다른 인덱스 두 개의 합이 `target`인 쌍 수; O(n²) 시간·O(1) 공간.

**분류** [[00 Maps/Exhaustive Search|Exhaustive Search]]
