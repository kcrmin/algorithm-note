---
type: template
tags: ["Search"]
category: exhaustive-search
categories: [exhaustive-search]
---

# Simulation

```java
static Point simulate(int[][] map, String commands) {
    int H = map.length, W = map[0].length;
    int[] dx = {1, 0, -1, 0}, dy = {0, 1, 0, -1};
    Point cur = new Point(0, 0);
    int dir = 0;
    for (char command : commands.toCharArray()) {
        if (command == 'R') dir = (dir + 1) % 4;
        else if (command == 'L') dir = (dir + 3) % 4; // -1 대신 +3: Java 음수 나머지 방지
        else {
            int nx = cur.x + dx[dir], ny = cur.y + dy[dir];
            if (nx < 0 || nx >= W || ny < 0 || ny >= H) continue;
            if (map[ny][nx] == 0) continue;
            cur.x = nx; // 검증 후 반영해야 실패한 이동을 되돌릴 필요 없음
            cur.y = ny;
        }
    }
    return cur;
}
```

`map[y][x]`: 1=길·0=벽, x=열·y=행; `Point`는 `java.awt.Point`; 시작 (0,0)·오른쪽, F=전진·R/L=회전. O(명령 수).

**분류** [[00 Maps/Exhaustive Search|Exhaustive Search]]
