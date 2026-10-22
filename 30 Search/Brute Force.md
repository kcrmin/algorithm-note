---
type: template
tags: ["Search"]
aliases: ["Exhaustive Search"]
---

# Brute Force

```java
static long countPairs(int[] numbers, long target) {
    long count = 0;
    for (int i = 0; i < numbers.length; i++) {
        for (int j = i + 1; j < numbers.length; j++) { // ①
            if ((long) numbers[i] + numbers[j] == target) count++;
        }
    }
    return count;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① j = i + 1 | i<j만 만들면 자기 자신과 (j,i) 중복이 사라진다. |
| 먼저 long으로 변환 | int끼리 더한 뒤 변환하면 이미 overflow가 날 수 있다. |
| 후보 생성 → 검사 → 집계 | 누락과 중복은 검사식보다 후보 생성 범위에서 먼저 결정된다. |

**예시**

```text
[1, 2, 2, 3], 합=4
인덱스 쌍  (0,3) ✓  (1,2) ✓
i=0 → j=1,2,3    i=1 → j=2,3
자기 자신과 역순 쌍은 만들지 않음 → 답 2
```

**적용** 가능한 후보 수를 전부 검사해도 시간 제한 안에 들어갈 때 사용한다.

**준비** 예시는 서로 다른 두 인덱스의 합이 target인 쌍의 개수다.

**주의** n²가 너무 크면 정렬·HashMap 등으로 후보를 줄여야 한다. O(n²) 시간, O(1) 공간.

[[Backtracking]] · [[Simulation]] · [[Two Pointers - Pair Sum]]
