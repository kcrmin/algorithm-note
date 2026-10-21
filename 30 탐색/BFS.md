---
type: template
tags: [탐색]
aliases: [Visited, Distance]
---

# BFS

같은 비용으로 이동할 때 시작점에서 각 칸까지의 최단거리를 구한다.

> **기억:** 시작을 큐에 → 하나 꺼냄 → 사방 확인 → 처음 가는 칸의 거리 기록 → 큐에 넣기.

**전제:** `1`은 길, `0`은 벽인 H×W 격자, 출발·도착은 길, 상하좌우 비용 동일. 시간·추가 공간 `O(HW)`.

## 템플릿

`Point`는 `x=열, y=행`, 배열은 `map[y][x]`다. `queue`에는 `(0,0)`, `dist[0][0]=1`을 넣고 시작한다. 나머지 거리는 0, `dx/dy`는 사방 이동이다.

```java
static int bfs(Queue<Point> queue) {
    while (!queue.isEmpty()) {
        Point cur = queue.poll();
        if (cur.x == W - 1 && cur.y == H - 1) {
            return dist[cur.y][cur.x];
        }
        for (int dir = 0; dir < 4; dir++) {
            int nx = cur.x + dx[dir];
            int ny = cur.y + dy[dir];
            if (nx < 0 || nx >= W || ny < 0 || ny >= H) continue;
            if (map[ny][nx] == 0 || dist[ny][nx] != 0) continue;
            dist[ny][nx] = dist[cur.y][cur.x] + 1;
            queue.offer(new Point(nx, ny));
        }
    }
    return -1;
}
```

## 왜 이렇게 쓰나

- 큐는 거리 1인 칸 다음에 거리 2인 칸을 꺼내므로 **처음 발견한 거리**가 최단이다.
- 방문 표시는 **넣기 직전** 거리 기록으로 한다. 꺼낼 때 표시하면 같은 칸이 여러 번 들어간다.
- 범위를 먼저 검사해야 `map[ny][nx]`를 안전하게 읽는다. 시작을 1로 두므로 답은 간선 수가 아닌 칸 수다.

**작게 추적:** 모두 길인 2×2: `(0,0):1 → (1,0),(0,1):2 → (1,1):3`. 두 경로가 만나도 도착 칸은 한 번만 넣는다.

> [!info]- 더 자세한 설명
> ### 기존 전체 구현의 개념과 적용 조건
>
> BFS는 시작점에서 간선 한 개로 닿는 곳, 두 개로 닿는 곳 순서로 탐색하는 방법이다. 큐의 선입선출 순서가 거리가 작은 층부터 처리하게 하므로 모든 이동 비용이 같을 때 처음 발견한 거리가 최단거리다.
>
> ## 적용 조건
>
> - 미로에서 최소 이동 횟수, 같은 비용의 간선으로 연결된 그래프에서 최단 거리를 구한다.
> - 이 템플릿은 `1`이 길, `0`이 벽인 직사각형 격자에서 상하좌우로 한 칸씩 이동한다.
> - 시작은 왼쪽 위 `(0, 0)`, 도착은 오른쪽 아래 `(W - 1, H - 1)`이다.
> - **모든 이동 비용이 같아야** 먼저 도착한 경로가 최단 경로가 된다. 비용이 제각각이면 다익스트라를 검토한다.
> - 높이 `H`, 너비 `W`일 때 시간 `O(HW)`, 추가 공간 `O(HW)`다. 한 칸을 한 번만 큐에 넣는다.
>
> ### 1. 좌표는 `x = 열`, `y = 행`, 배열은 `map[y][x]`
>
> `Point(2, 1)`은 **오른쪽으로 두 칸, 아래로 한 칸**인 위치다. 배열에서는 `map[1][2]`다.
> `H = 행 개수`, `W = 열 개수`이므로 `x`는 `W`, `y`는 `H`와 비교한다.
> `dx[dir]`와 `dy[dir]`를 같은 `dir`로 읽어 하나의 이동을 만든다.
>
> 예를 들어 현재가 `(1, 1)`이고 `dir = 0`이면 `nx = 1 + 1 = 2`, `ny = 1 + 0 = 1`로 오른쪽이다.
> `dir = 1`이면 `(1, 2)`로 아래쪽이다. 방향을 조사하는 순서는 최단 거리 값에 영향을 주지 않는다.
>
> ### 2. 큐는 가까운 칸부터 처리한다
>
> 다음 지도에서 출발점은 `(0, 0)`, 도착점은 `(2, 2)`다. 표 안의 좌표도 항상 `(x, y)`다.
>
> ```text
>        x=0 1 2
> y=0     1 1 0
> y=1     0 1 1
> y=2     1 1 1
> ```
>
> 처음 `dist[0][0] = 1`, 큐는 `[(0,0)]`이다. 큐의 왼쪽을 먼저 꺼낸다.
>
> | 단계 | 꺼낸 칸 | 새로 기록한 칸과 거리 | 처리 후 큐 |
> | --- | --- | --- | --- |
> | 1 | `(0,0)` | `(1,0) = 2` | `[(1,0)]` |
> | 2 | `(1,0)` | `(1,1) = 3` | `[(1,1)]` |
> | 3 | `(1,1)` | `(2,1) = 4`, `(1,2) = 4` | `[(2,1), (1,2)]` |
> | 4 | `(2,1)` | `(2,2) = 5` | `[(1,2), (2,2)]` |
> | 5 | `(1,2)` | `(0,2) = 5` | `[(2,2), (0,2)]` |
> | 6 | `(2,2)` | 도착했으므로 `5` 반환 | 탐색 종료 |
>
> 거리 `4`인 두 칸을 모두 처리한 다음 거리 `5`인 칸을 꺼낸다.
> 이처럼 큐의 선입선출 순서가 거리가 작은 층부터 탐색하도록 만든다.
>
> ### 3. 방문 표시는 큐에 넣을 때 한다
>
> `dist[ny][nx] = ...`를 **`queue.offer(...)` 바로 앞에서** 실행한다.
> 위 예시에서 `(2,2)`는 `(2,1)`에서 이미 거리 `5`로 기록되었다.
> 나중에 `(1,2)`가 같은 칸을 발견해도 `dist[2][2] != 0`이라 다시 넣지 않는다.
>
> 꺼낼 때 방문 표시를 하면, 아직 큐에 대기 중인 칸을 다른 이웃도 다시 넣을 수 있다.
> 처음 발견한 순간 거리를 확정하면 중복 삽입과 거리 덮어쓰기를 함께 막는다.
>
> ### 4. `dist`가 `visited` 역할까지 한다
>
> - `map[y][x] == 0`: 원래 벽이다.
> - `map[y][x] == 1 && dist[y][x] == 0`: 길이지만 아직 발견하지 않았다.
> - `dist[y][x] > 0`: 이미 발견했고, 그 값이 시작부터 센 최단 칸 수다.
>
> 따라서 이 코드에는 `boolean[][] visited`가 따로 필요 없다.
> 거리 없이 연결 여부만 필요하다면 `visited`만 쓰는 편이 목적에 맞다.
> 이동 횟수를 세어 시작 거리를 `0`으로 바꿀 때는 미방문도 `0`이면 구별할 수 없다.
> 그 경우 `dist`를 `-1`로 채우고, 시작만 `0`으로 두며, 미방문 조건도 `dist[ny][nx] == -1`로 바꾼다.
>
> ### 5. 범위를 먼저 검사해야 배열에 접근할 수 있다
>
> `(0,0)`에서 왼쪽을 보면 `nx = -1`이 나온다. `map[0][-1]`은 존재하지 않는다.
> 그래서 **범위 검사 → 벽·방문 검사 → 거리 기록 → 큐 삽입** 순서다.
> `map[ny][nx]`를 먼저 읽고 나중에 범위를 검사하면 이미 예외가 발생한다.
>
> ### 6. 칸 수와 이동 횟수는 1 차이 난다
>
> 예시 경로는 `(0,0) → (1,0) → (1,1) → (2,1) → (2,2)`다.
> 칸은 `5`개이고 이동은 `4`번이다. 이 템플릿은 시작 칸을 `1`로 센다.
> 길인 칸 하나뿐인 `1×1` 지도는 시작이 곧 도착이므로 `1`을 반환한다.
> 길인 도착점이 있어도 큐가 빌 때까지 도착하지 못하면 `-1`이다.
>
> ### 주의점
>
> - **문제에서 세는 값**: 칸 수인지 이동 횟수인지 먼저 확인한다. 이 템플릿의 성공 결과에서 `1`을 빼면 이동 횟수다. `-1`은 그대로 둔다.
> - **좌표 입력 순서**: 문제가 `(행, 열)`을 주면 `y = 행`, `x = 열`로 받아 `new Point(x, y)`에 넣는다.
> - **이동 규칙**: 대각선도 가능하면 `dx`, `dy`와 반복 횟수를 함께 바꾼다.
> - **재호출**: `solution()`마다 `map`, `H`, `W`, `dist`를 새 입력 기준으로 설정한다. 정적 필드의 이전 거리를 재사용하지 않는다.
> - **도착점 처리**: 범위 밖·벽·방문한 칸은 큐에 넣지 않으므로, 큐에서 꺼낸 도착점의 거리는 유효하다.
> - **가중치**: 이동 비용이 `1, 5, 10`처럼 다르면 이 방문 규칙으로 최단 거리를 확정할 수 없다.
> - **DFS와 선택**: 갈 수 있는지만 확인하면 DFS도 가능하지만, 같은 비용의 최단 거리는 BFS를 먼저 떠올린다.
>
> **관련 개념:** [[스택과 큐]] · [[다익스트라]] · [[DFS와 그래프]]

> [!example]- 준비 코드 · 전체 구현
> ### 위 핵심의 준비 코드와 실행 예
>
> 필드·초기화·헬퍼를 포함하며, main의 숫자 사례를 그대로 실행할 수 있다. 새 입력을 풀 때는 배열·큐·방문 상태를 새로 준비한다.
>
> ```java
> import java.util.*;
>
> class GraphKernel0 {
>     static int H, W;
>     static int[][] map, dist;
>     static final int[] dx = {1, 0, -1, 0};
>     static final int[] dy = {0, 1, 0, -1};
>     static class Point {
>         int x, y;
>         Point(int x, int y) { this.x = x; this.y = y; }
>     }
>
>     static int bfs(Queue<Point> queue) {
>         while (!queue.isEmpty()) {
>             Point cur = queue.poll();
>             if (cur.x == W - 1 && cur.y == H - 1) {
>                 return dist[cur.y][cur.x];
>             }
>             for (int dir = 0; dir < 4; dir++) {
>                 int nx = cur.x + dx[dir];
>                 int ny = cur.y + dy[dir];
>                 if (nx < 0 || nx >= W || ny < 0 || ny >= H) continue;
>                 if (map[ny][nx] == 0 || dist[ny][nx] != 0) continue;
>                 dist[ny][nx] = dist[cur.y][cur.x] + 1;
>                 queue.offer(new Point(nx, ny));
>             }
>         }
>         return -1;
>     }
>
>     static void check(boolean success, String message) {
>         if (!success) throw new AssertionError(message);
>     }
>
>     public static void main(String[] args) {
>         map = new int[][] {{1, 1}, {1, 1}};
>         H = 2;
>         W = 2;
>         dist = new int[H][W];
>         dist[0][0] = 1;
>         Queue<Point> queue = new ArrayDeque<>();
>         queue.offer(new Point(0, 0));
>         check(bfs(queue) == 3, "2x2 shortest includes start");
>         map = new int[][] {{1, 0, 1}, {0, 1, 1}};
>         H = 2;
>         W = 3;
>         dist = new int[H][W];
>         dist[0][0] = 1;
>         queue.offer(new Point(0, 0));
>         check(bfs(queue) == -1, "unreachable");
>         map = new int[][] {{1}};
>         H = W = 1;
>         dist = new int[][] {{1}};
>         queue.clear();
>         queue.offer(new Point(0, 0));
>         check(bfs(queue) == 1, "one cell");
>         System.out.println("PASS BFS");
>     }
> }
> ```
>
> ### 기존 전체 구현
>
>
> 프로그래머스 「게임 맵 최단거리」처럼 **시작 칸을 포함한 칸 수**를 반환한다. 도달할 수 없으면 `-1`이다.
> 입력은 비어 있지 않은 직사각형 `int[][]`라고 가정한다. 원본 `maps`는 바꾸지 않는다.
>
> ```java
> import java.util.ArrayDeque;
> import java.util.Queue;
>
> public class Solution {
>     static int H, W;
>     static int[][] map;
>     static int[][] dist;
>
>     // 오른쪽, 아래, 왼쪽, 위
>     static final int[] dx = {1, 0, -1, 0};
>     static final int[] dy = {0, 1, 0, -1};
>
>     static class Point {
>         int x;
>         int y;
>
>         Point(int x, int y) {
>             this.x = x;
>             this.y = y;
>         }
>     }
>
>     public int solution(int[][] maps) {
>         map = maps;
>         H = map.length;
>         W = map[0].length;
>         dist = new int[H][W];
>
>         if (map[0][0] == 0 || map[H - 1][W - 1] == 0) {
>             return -1;
>         }
>
>         return bfs();
>     }
>
>     private static int bfs() {
>         Queue<Point> queue = new ArrayDeque<>();
>         dist[0][0] = 1;
>         queue.offer(new Point(0, 0));
>
>         while (!queue.isEmpty()) {
>             Point cur = queue.poll();
>
>             if (cur.x == W - 1 && cur.y == H - 1) {
>                 return dist[cur.y][cur.x];
>             }
>
>             for (int dir = 0; dir < 4; dir++) {
>                 int nx = cur.x + dx[dir];
>                 int ny = cur.y + dy[dir];
>
>                 if (nx < 0 || nx >= W || ny < 0 || ny >= H) {
>                     continue;
>                 }
>                 if (map[ny][nx] == 0 || dist[ny][nx] != 0) {
>                     continue;
>                 }
>
>                 dist[ny][nx] = dist[cur.y][cur.x] + 1;
>                 queue.offer(new Point(nx, ny));
>             }
>         }
>
>         return -1;
>     }
> }
> ```
