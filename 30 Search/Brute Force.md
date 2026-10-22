---
type: template
tags: ["Search"]
aliases: ["Exhaustive Search"]
---

# Brute Force

```java
// 서로 다른 두 인덱스의 합이 target인 쌍을 센다.
// 모든 후보를 확인하므로 n이 작을 때 사용한다.
static long countPairs(int[] numbers, long target) {
    long count = 0;
    for (int i = 0; i < numbers.length; i++) {
        // i<j만 생성: 자기 자신과 역순 쌍 중복 제거
        for (int j = i + 1; j < numbers.length; j++) {
            // 더하기 전에 long으로 바꿔 int overflow 방지
            if ((long) numbers[i] + numbers[j] == target) count++;
        }
    }
    return count;
}
```

**예시**

```text
[1, 2, 2, 3], 합=4
인덱스 쌍  (0,3) ✓  (1,2) ✓
i=0 → j=1,2,3    i=1 → j=2,3
자기 자신과 역순 쌍은 만들지 않음 → 답 2
```

**주의** n²가 너무 크면 정렬·HashMap 등으로 후보를 줄여야 한다. O(n²) 시간, O(1) 공간.

[[Backtracking]] · [[Simulation]] · [[Two Pointers - Pair Sum]]
