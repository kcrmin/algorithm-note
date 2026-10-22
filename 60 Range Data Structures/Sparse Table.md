---
type: template
tags: ["DataStructures"]
aliases: ["Sparse_Table"]
---

# Sparse Table

```java
// 필드 int[][] table; 고정된 비어 있지 않은 배열로 build(arr) 후
// min(left,right)를 호출한다.
// 0-index·양끝 포함. table[k][i]는 i부터 길이 2^k의 최솟값이다.
void build(int[] arr) {
    int n = arr.length;
    // 길이 n 이하인 1,2,4,... 구간을 모두 저장한다.
    int levels = 32 - Integer.numberOfLeadingZeros(n);
    table = new int[levels][n];
    table[0] = arr.clone();
    for (int k = 1; k < levels; k++) {
        int half = 1 << (k - 1);
        for (int i = 0; i + (1 << k) <= n; i++) {
            table[k][i] = Math.min(table[k - 1][i],
                                   // 길이 절반인 두 구간의 min을 합친다.
                                   table[k - 1][i + half]);
        }
    }
}
int min(int left, int right) {
    int length = right - left + 1;
    // 최고 1비트 위치 = floor(log2 길이).
    int k = 31 - Integer.numberOfLeadingZeros(length);
    return Math.min(table[k][left],
                    // 양끝 구간이 겹쳐도 min은 같아 전체를 O(1)에 덮는다.
                    table[k][right - (1 << k) + 1]);
}
```

**예시**

```text
질의 [1..6], 길이 6 → 가장 큰 2의 거듭제곱은 4
값       9 [5 7 2 8 3 6] 4
왼쪽 4칸   [5 7 2 8]     → 2
오른쪽 4칸     [2 8 3 6] → 2
겹쳐도 min(2,2)=2        → 중복이 답을 바꾸지 않음
```

**주의** 이 O(1) 질의는 min/max/GCD처럼 중복해도 같은 연산용이다. 합에는 못 쓴다. 전처리·공간 O(N log N)·질의 O(1).

[[Segment Tree]] · [[GCD and LCM]]
