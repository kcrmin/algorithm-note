---
type: template
tags: ["Search"]
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
        else if (command == 'L') dir = (dir + 3) % 4; // ①
        else {
            int nx = cur.x + dx[dir], ny = cur.y + dy[dir];
            if (nx < 0 || nx >= W || ny < 0 || ny >= H) continue;
            if (map[ny][nx] == 0) continue;
            cur.x = nx; // ②
            cur.y = ny;
        }
    }
    return cur;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 왼쪽은 +3 | Java의 음수 나머지를 피하며 0→3으로 회전한다. |
| ② 검사 후 반영 | 잘못된 이동에서 이전 상태를 다시 복구할 필요가 없다. |

**예시**

```text
방향 번호: 오른쪽0 → 아래1 → 왼쪽2 → 위3
모두 길인 2×2, 시작 (0,0), 방향 0
명령 FRF : (0,0) → (1,0) → 방향1 → (1,1)
벽/범위 밖으로 F → 위치 유지
```

**적용** 명령대로 상태가 바뀌는 문제에서 다음 상태를 계산한 뒤 유효한 변화만 반영한다.

**준비** `map[y][x]`는 1=길. `Point`는 `java.awt.Point`. 명령은 F/R/L, 반환은 최종 위치.

**주의** 동시에 움직이는 문제는 이전/다음 상태를 분리해야 한다. O(명령 수) 시간, O(1) 공간.

[[Brute Force]] · [[BFS]]
