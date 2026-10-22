---
type: template
tags: ["DataStructures"]
category: prefix-sums
categories: [prefix-sums]
---

# 2D Prefix Sum

```text
          0          x1       x2+1
       0  +----------+--------+
          |    A     |   B    |
      y1  +----------+--------+
          |    C     |  want  |
    y2+1  +----------+--------+

want = 전체 - (A+B) - (A+C) + A
                              ↑ 두 번 뺀 겹침 복구
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Pad** | // 맨 위와 맨 왼쪽에 합이 0인 경계를 둔다. |
| **Build** | // 위쪽 합과 왼쪽 합을 더하고, 겹치는 왼쪽 위를 한 번 뺀다. |
| **Query** | // 전체에서 위쪽과 왼쪽을 빼고, 두 번 뺀 모서리를 다시 더한다. |

## Java

```java
long[][] prefix = new long[H + 1][W + 1];

for (int y = 0; y < H; y++) {
    for (int x = 0; x < W; x++) {
        prefix[y + 1][x + 1] = matrix[y][x]
                + prefix[y][x + 1]
                + prefix[y + 1][x]
                - prefix[y][x];
    }
}

return prefix[y2 + 1][x2 + 1]
        - prefix[y1][x2 + 1]
        - prefix[y2 + 1][x1]
        + prefix[y1][x1];
```

고정 행렬 · `0 ≤ y1 ≤ y2 < H`, `0 ≤ x1 ≤ x2 < W` · 전처리 `O(HW)`, 조회 `O(1)`

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `matrix[y][x]` | 첫 인덱스는 행 y, 두 번째는 열 x. H는 행 수, W는 열 수다. |
| `prefix[y][x]` | 앞 y행 × 앞 x열의 합. 0행·0열이 있어 시작 좌표가 0이어도 같은 식을 쓴다. |
| 조회 부호 `+ − − +` | 전체에서 위·왼쪽을 빼면 모서리는 두 번 빠진다. 마지막에 한 번 더한다. |
| 끝 좌표만 `+ 1` | y2행·x2열을 포함하려면 각각 한 칸 뒤의 누적 합을 조회한다. |

---

**분류** [[00 Maps/Prefix Sums|Prefix Sums]]

**사용 원리** [[20 Ranges/Prefix Sum|Prefix Sum]]
