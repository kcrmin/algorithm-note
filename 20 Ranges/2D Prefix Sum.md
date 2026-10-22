---
type: template
tags: ["DataStructures"]
---

# 2D Prefix Sum

```java
// 값이 바뀌지 않는 격자. H=행 수, W=열 수, matrix[y][x].
// x=열, y=행. (x1,y1)~(x2,y2) 양 끝 포함.
// 0행·0열을 비워 두면 첫 행·열에도 같은 식을 쓸 수 있다.
long[][] prefix = new long[H + 1][W + 1];
for (int y = 0; y < H; y++) {
    for (int x = 0; x < W; x++) {
        // 위+왼쪽에 중복된 모서리를 한 번 빼서 쌓는다.
        prefix[y + 1][x + 1] = matrix[y][x]
                + prefix[y][x + 1] + prefix[y + 1][x]
                - prefix[y][x];
    }
}
// 전체-위-왼쪽에서 모서리가 두 번 빠져 한 번 더한다.
// 끝 좌표는 포함하므로 +1, 시작은 직전 영역을 빼므로 그대로다.
return prefix[y2 + 1][x2 + 1]
        - prefix[y1][x2 + 1] - prefix[y2 + 1][x1]
        + prefix[y1][x1];
```

**예시**

```text
matrix       prefix (0행·0열 비움)
1  2         0  0  0
3 [4]        0  1  3
             0  4 10
[4]의 합 = 전체10 − 위3 − 왼쪽4 + 겹침1 = 4
```

**주의** x=열, y=행. 전처리·공간 O(HW), 조회 O(1). prefix는 한 번만 만든다.

[[Prefix Sum]] · [[Fenwick Tree]] · [[Segment Tree]]
