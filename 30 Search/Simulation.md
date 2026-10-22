---
type: template
tags: ["Search"]
category: exhaustive-search
categories: [exhaustive-search]
---

# Simulation

```text
방향 인덱스: 0 →   1 ↓   2 ←   3 ↑

시작 (0,0) →
F: (1,0) →
R: (1,0) ↓
F: (1,1) ↓     이동 후보가 벽이면 제자리

회전 → 후보 좌표 → 검사 → 위치 반영
```

| Step | Pseudocode |
|---|---|
| Turn | // 오른쪽이면 방향을 한 칸, 왼쪽이면 반대쪽으로 한 칸 돌린다. |
| Try | // 현재 위치와 방향으로 다음 좌표를 계산한다. |
| Check | // 범위를 벗어나거나 벽이면 이번 이동을 버린다. |
| Commit | // 검사를 통과한 좌표만 현재 위치에 반영한다. |

## Java

```java
static Point simulate(int[][] map, String commands) {
    int H = map.length, W = map[0].length;
    int[] dx = {1, 0, -1, 0}, dy = {0, 1, 0, -1};
    Point cur = new Point(0, 0);
    int dir = 0;

    for (char command : commands.toCharArray()) {
        if (command == 'R') {
            dir = (dir + 1) % 4;
        } else if (command == 'L') {
            dir = (dir + 3) % 4;
        } else {
            int nx = cur.x + dx[dir], ny = cur.y + dy[dir];
            if (nx < 0 || nx >= W || ny < 0 || ny >= H) {
                continue;
            }

            if (map[ny][nx] == 0) {
                continue;
            }

            cur.x = nx;
            cur.y = ny;
        }
    }

    return cur;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| `(dir+3)%4` | 왼쪽은 -1과 같다. Java의 음수 나머지를 피하려고 +3을 쓴다. |
| 후보 계산 → 검사 → 반영 | 먼저 이동시키면 실패했을 때 좌표를 다시 복구해야 한다. |
| x=열, y=행 | 배열 접근은 map[y][x]. 아래쪽 이동은 y가 증가한다. |
| 명령 집합 | R·L 이외를 전진으로 처리하므로 입력 명령은 F/R/L로 제한한다. |

비어 있지 않은 `map`(1=길, 0=벽), 시작 (0,0)은 길. `Point`는 `java.awt.Point`. 반환: 최종 좌표. **O(명령 수)**.

**분류** [[00 Maps/Exhaustive Search|Exhaustive Search]]
