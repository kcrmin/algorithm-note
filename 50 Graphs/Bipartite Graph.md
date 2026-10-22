---
type: template
tags: ["Graphs"]
---

# Bipartite Graph

```java
// 무방향 graph는 List<Integer>[]. color=0은 미방문, +1/-1은 두 그룹.
static boolean isBipartite(int n) {
    // 끊긴 다른 덩어리에도 홀수 사이클이 있을 수 있음
    for (int start = 0; start < n; start++) {
        if (color[start] != 0) continue;
        Queue<Integer> queue = new ArrayDeque<>();
        color[start] = 1;
        queue.offer(start);
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            for (int next : graph[cur]) {
                // 같은 그룹끼리 간선이 생기면 두 색 배정 모순
                if (color[next] == color[cur]) return false;
                if (color[next] != 0) continue;
                color[next] = -color[cur]; // 간선을 건널 때마다 반대 그룹이어야 함
                queue.offer(next);
            }
        }
    }
    return true;
}
```

**예시**

```text
0—1—2—3   색 + - + -   ✓
0—1        0:+ → 1:- → 2:+ → 0:- 충돌
 \2/       홀수 사이클은 두 색으로 칠할 수 없음
```

**주의** 이분 판정과 최대 배정은 서로 다른 문제다. O(V+E) 시간·공간.

[[BFS]] · [[Bipartite Matching]]
