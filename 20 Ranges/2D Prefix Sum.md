---
type: template
tags: ["DataStructures"]
---

# 2D Prefix Sum

**언제** 값이 바뀌지 않는 격자에서 직사각형 합을 여러 번 묻는다.

```text
matrix       prefix (0행·0열 비움)
1  2         0  0  0
3 [4]        0  1  3
             0  4 10
[4]의 합 = 전체10 − 위3 − 왼쪽4 + 겹침1 = 4
```

**코드** `matrix[y][x]`. `(x1,y1)~(x2,y2)`를 포함하는 long 합을 반환한다.

```java
long[][] prefix = new long[H + 1][W + 1];
for (int y = 0; y < H; y++) {
    for (int x = 0; x < W; x++) {
        prefix[y + 1][x + 1] = matrix[y][x]
                + prefix[y][x + 1] + prefix[y + 1][x]
                - prefix[y][x];
    }
}
return prefix[y2 + 1][x2 + 1]
        - prefix[y1][x2 + 1] - prefix[y2 + 1][x1]
        + prefix[y1][x1];
```

| 암기 포인트 | 이유 |
|---|---|
| 쌓기: `위 + 왼쪽 − 겹침` | 위·왼쪽을 더하면 모서리가 두 번 들어가 한 번 뺀다. |
| 조회: `전체 − 위 − 왼쪽 + 겹침` | 두 번 빠진 모서리를 한 번 돌려놓는다. |
| 끝만 `+1` | 양 끝 포함 좌표라 끝 원소까지 쌓은 합에서 시작 직전 합을 뺀다. |

**주의** x=열, y=행. 전처리·공간 O(HW), 조회 O(1). prefix는 한 번만 만든다.

[[Prefix Sum]] · [[Fenwick Tree]] · [[Segment Tree]]
