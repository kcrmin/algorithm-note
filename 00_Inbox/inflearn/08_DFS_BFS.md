---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# 섹션 8 - DFS BFS

## 1. DFS (Depth First Search) - 깊이 우선 탐색 기초

```java
import java.util.*;

public class Main {
    public static class Point {
        int x;
        int y;
        
        public Point(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
    
    static int[] dx = {1, 0, -1, 0};
    static int[] dy = {0, -1, 0, 1};
    static int n;
    static int[][] board;
    static boolean[][] visited;
    
    public static void DFS(int x, int y) {
        if (board[y][x] == 1) return;
        else {
            visited[y][x] = true;
            for (int i = 0; i < 4; i++) {
                int nx = point.x + dx[i];
                int ny = point.y + dy[i];
                board[point.y][point.x] = 0;
                if (nx >= 0 && nx < n && ny >= 0 && ny < n && !visited[ny][nx] && board[ny][nx] == 1) DFS(nx, ny);
        }
    }
}
```
> [08-04 중복순열 구하기](../../src/inflearn/section08_dfs_bfs/INF_S08_P04.java) 참고

> [08-06 순열 구하기](../../src/inflearn/section08_dfs_bfs/INF_S08_P06.java) 참고

> [08-09 조합 구하기](../../src/inflearn/section08_dfs_bfs/INF_S08_P09.java) 참고


## 2. BFS (Breadth First Search) - 너비 우선 탐색 기초

```java
import java.util.*;

public class Main {
    public static class Point {
        int x;
        int y;

        public Point(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
    
    static int[] dx = {1, 0, -1, 0};
    static int[] dy = {0, -1, 0, 1};
    static int[][] dis;

    public static void BFS() {
        Queue<Point> queue = new ArrayDeque<>();

        while (!queue.isEmpty()) {
            int len = queue.size();
            for (int i = 0; i < len; i++) {
                Point curr = queue.poll();
                int x = curr.x;
                int y = curr.y;

                for (int i = 0; i < 4; i++) {
                    int nx = x + dx[i];
                    int ny = y + dy[i];
                    
                    if (nx >= 1 && ny >= 1 && nx <= 7 && ny <= 7 && dis[ny][nx] == 0 && board[ny][nx] == 0) {
                        dis[ny][nx] = dis[y][x] + 1;
                        queue.offer(new Point(nx, ny));
                    }
                }
            }
        }
        
        return -1;
    }
}
```
> int[][] dis: 사용시 queue.size()를 사용하지 않아 효율적


## 3. 주의 포인트
|       코드 / 개념        | 요약              |
|:--------------------:|:----------------|
|     visited_초기화      | 테스트케이스마다_새로_초기화 |
| Queue는 ArrayDeque 사용 | LinkedList보다 빠름 |


## **핵심 요약**
- DFS (깊이 우선): 연결 요소, ***조합/순열** 탐색
  - Stack 사용
- BFS (너비 우선): 최단 거리
  - Queue 사용
- 둘 다 visited 필수

## 문제 다시 풀기
### **[Inflearn](../../src/inflearn/section08_dfs_bfs)**
| 분류  | 문제 이름                                                                                           | 포인트 메모                   |
|:---:|:------------------------------------------------------------------------------------------------|:-------------------------|
| 필수  | [08-04 중복순열 구하기](../../src/inflearn/section08_dfs_bfs/INF_S08_P04.java)                         | 필수 지식                    |
| 필수  | [08-06 순열 구하기](../../src/inflearn/section08_dfs_bfs/INF_S08_P06.java)                           | 필수 지식                    |
| 필수  | [08-09 조합 구하기](../../src/inflearn/section08_dfs_bfs/INF_S08_P09.java)                           | 필수 지식                    |
| 응용  | [08-01 합이 같은 부분집합(DFS : 아마존 인터뷰)](../../src/inflearn/section08_dfs_bfs/INF_S08_P01.java)        | int[] checked 쓰기         |
| 응용  | [08-02 바둑이 승차(DFS)](../../src/inflearn/section08_dfs_bfs/INF_S08_P02.java)                      | 좋은 문제, sum 계속 넘기기        |
| 응용  | [08-03 최대점수 구하기(DFS)](../../src/inflearn/section08_dfs_bfs/INF_S08_P03.java)                    | 좋은 문제, new int[] 2개 사용하기 |
| 기초  | [08-05 동전교환](../../src/inflearn/section08_dfs_bfs/INF_S08_P05.java)                             | 좋은 문제                    |
| 감잡기 | [08-07 조합의 경우수(메모이제이션)](../../src/inflearn/section08_dfs_bfs/INF_S08_P07.java)                  | 필수 지식                    |
| 고난도 | [08-08 수열 추측하기](../../src/inflearn/section08_dfs_bfs/INF_S08_P08.java)                          | nCr 지식 필수                |
| 응용  | [08-10 미로탐색(DFS)](../../src/inflearn/section08_dfs_bfs/INF_S08_P10.java)                        | 미로 탐색                    |
| 응용  | [08-11 미로의 최단거리 통로(BFS)](../../src/inflearn/section08_dfs_bfs/INF_S08_P11.java)                 | 미로 탐색, dis 사용            |
| 감잡기 | [08-12 토마토(BFS 활용)](../../src/inflearn/section08_dfs_bfs/INF_S08_P12.java)                      | 좋은 문제, dis 사용            |
| 감잡기 | [08-13 섬나라 아일랜드](../../src/inflearn/section08_dfs_bfs/INF_S08_P13.java)                         | 좋은 문제, DFS/BFS 둘 다 도전    |
| 고난도 | [08-15 피자 배달 거리(삼성 SW역량평가 기출문제 : DFS활용)](../../src/inflearn/section08_dfs_bfs/INF_S08_P15.java) | 매우 어려운 문제, 완벽한 순열 이해     |