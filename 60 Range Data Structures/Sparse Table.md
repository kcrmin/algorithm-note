---
type: template
tags: ["DataStructures"]
aliases: ["Sparse_Table"]
---

# Sparse Table

```java
void build(int[] arr) {
    int n = arr.length;
    int levels = 32 - Integer.numberOfLeadingZeros(n);
    table = new int[levels][n];
    table[0] = arr.clone();
    for (int k = 1; k < levels; k++) {
        int half = 1 << (k - 1);
        for (int i = 0; i + (1 << k) <= n; i++) {
            table[k][i] = Math.min(table[k - 1][i],
                                   table[k - 1][i + half]); // ①
        }
    }
}
int min(int left, int right) {
    int length = right - left + 1;
    int k = 31 - Integer.numberOfLeadingZeros(length);
    return Math.min(table[k][left],
                    table[k][right - (1 << k) + 1]); // ②
}
```

| 암기 포인트 | 이유 |
|---|---|
| `table[k][i]` | i부터 길이 2ᵏ의 최솟값. ①은 절반 두 개를 합친다. |
| ② 양끝에서 2ᵏ칸 | 두 구간이 전체 질의를 덮고, 겹쳐도 최솟값은 같다. |
| `31-leadingZeros` | 양수의 최고 1비트 위치가 ⌊log₂ 길이⌋다. |

**예시**

```text
질의 [1..6], 길이 6 → 가장 큰 2의 거듭제곱은 4
값       9 [5 7 2 8 3 6] 4
왼쪽 4칸   [5 7 2 8]     → 2
오른쪽 4칸     [2 8 3 6] → 2
겹쳐도 min(2,2)=2        → 중복이 답을 바꾸지 않음
```

**적용** 값이 바뀌지 않는 배열에서 구간 최솟값을 많이 물을 때.

**준비** 필드 `int[][] table;`. `build(arr)` 후 `min(left,right)` 호출. **0-index·양끝 포함**, 비어 있지 않은 배열·질의.

**주의** 이 O(1) 질의는 min/max/GCD처럼 중복해도 같은 연산용이다. 합에는 못 쓴다. 전처리·공간 O(N log N)·질의 O(1).

[[Segment Tree]] · [[GCD and LCM]]
