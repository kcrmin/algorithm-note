---
type: template
tags: ["Search"]
---

# Simulation

```java
// map[y][x]: 1=길, 0=벽. Point는 java.awt.Point. 명령은 F=전진, R/L=회전.
// 방향 번호 0,1,2,3 = 오른쪽,아래,왼쪽,위. 시작은 (0,0), 오른쪽.
static Point simulate(int[][] map, String commands) {
    int H = map.length, W = map[0].length;
    int[] dx = {1, 0, -1, 0}, dy = {0, 1, 0, -1};
    Point cur = new Point(0, 0);
    int dir = 0;
    for (char command : commands.toCharArray()) {
        if (command == 'R') dir = (dir + 1) % 4;
        // -1 대신 +3으로 Java의 음수 나머지 방지
        else if (command == 'L') dir = (dir + 3) % 4;
        else {
            // 다음 상태를 먼저 계산하고 검증
            int nx = cur.x + dx[dir], ny = cur.y + dy[dir];
            if (nx < 0 || nx >= W || ny < 0 || ny >= H) continue;
            if (map[ny][nx] == 0) continue;
            cur.x = nx; // 유효할 때만 반영하므로 실패한 이동을 되돌릴 필요 없음
            cur.y = ny;
        }
    }
    return cur;
}
```

**예시**

```text
방향 번호: 오른쪽0 → 아래1 → 왼쪽2 → 위3
모두 길인 2×2, 시작 (0,0), 방향 0
명령 FRF : (0,0) → (1,0) → 방향1 → (1,1)
벽/범위 밖으로 F → 위치 유지
```

**주의** 동시에 움직이는 문제는 이전/다음 상태를 분리해야 한다. O(명령 수) 시간, O(1) 공간.

[[Brute Force]] · [[BFS]]
