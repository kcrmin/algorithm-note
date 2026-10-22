---
type: template
tags: ["Graphs"]
---

# Bipartite Graph

**언제** 모든 간선이 서로 다른 두 그룹을 잇도록 정점을 두 색으로 나눌 수 있는지 판정한다.

```text
0—1—2—3   색 + - + -   ✓
0—1        0:+ → 1:- → 2:+ → 0:- 충돌
 \2/       홀수 사이클은 두 색으로 칠할 수 없음
```

**코드** 무방향 `List<Integer>[] graph`. `color`는 0=미방문, +1/-1=두 색.

```java
static boolean isBipartite(int n) {
    for (int start = 0; start < n; start++) {
        if (color[start] != 0) continue;
        Queue<Integer> queue = new ArrayDeque<>();
        color[start] = 1;
        queue.offer(start);
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            for (int next : graph[cur]) {
                if (color[next] == color[cur]) return false; // ①
                if (color[next] != 0) continue;
                color[next] = -color[cur];
                queue.offer(next);
            }
        }
    }
    return true;
}
```

| 암기 포인트 | 이유 |
|---|---|
| 이웃은 반대 색 | 간선 하나를 건널 때마다 그룹이 바뀌어야 한다. |
| ① 같은 색끼리 연결되면 실패 | 다른 길로 돌아왔을 때 요구하는 색이 모순된다. |
| 전체 시작점 확인 | 끊긴 연결 요소에도 홀수 사이클이 있을 수 있다. |

**주의** 이분 판정과 최대 배정은 서로 다른 문제다. O(V+E) 시간·공간.

[[BFS]] · [[Bipartite Matching]]
