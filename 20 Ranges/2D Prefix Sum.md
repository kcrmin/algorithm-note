---
type: template
tags: ["DataStructures"]
category: prefix-sums
categories: [prefix-sums]
---

# 2D Prefix Sum

```java
// pad: 0행·0열을 비워 경계도 같은 식으로 처리
long[][] prefix = new long[H + 1][W + 1];
for (int y = 0; y < H; y++) {
    for (int x = 0; x < W; x++) {
        // build: 위 + 왼쪽 - 겹친 모서리
        prefix[y + 1][x + 1] = matrix[y][x]
                + prefix[y][x + 1] + prefix[y + 1][x]
                - prefix[y][x];
    }
}
return prefix[y2 + 1][x2 + 1] // query: 전체 - 위 - 왼쪽 + 겹침; 끝 좌표 포함
        - prefix[y1][x2 + 1] - prefix[y2 + 1][x1]
        + prefix[y1][x1];
```

**조건** 고정 matrix[y][x], H=행·W=열, 양 끝 포함. 전처리 O(HW), 조회 O(1).

**분류** [[00 Maps/Prefix Sums|Prefix Sums]]

**사용 원리** [[20 Ranges/Prefix Sum|Prefix Sum]]
