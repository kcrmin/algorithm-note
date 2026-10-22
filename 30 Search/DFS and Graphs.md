---
type: template
tags: ["Search"]
aliases: ["DFS", "Depth-First Search"]
---

# DFS and Graphs

```java
static void dfs(int cur) {
    visited[cur] = true; // ①
    for (int next : graph[cur]) {
        if (!visited[next]) dfs(next);
    }
}

static int countComponents(int n) {
    int count = 0;
    for (int start = 0; start < n; start++) {
        if (visited[start]) continue;
        count++; // ②
        dfs(start);
    }
    return count;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 들어오자마자 표시 | 0→1→0처럼 되돌아오는 간선에서 재귀를 멈춘다. |
| ② 전체 정점에서 시작 확인 | DFS 한 번으로는 다른 덩어리·고립 정점에 닿지 못한다. |

**예시**

```text
0—1—2    3—4    5
DFS(0): {0,1,2}
DFS(3): {3,4}   DFS(5): {5}
새 DFS 시작 3번 = 연결 요소 3개
```

**적용** 무방향 Graph에서 연결된 정점을 끝까지 방문하거나 연결 요소 수를 센다.

**준비** `List<Integer>[] graph`는 양방향 인접 리스트. `visited`는 모두 false.

**주의** 최단거리 용도가 아니다. 일자 Graph는 StackOverflow에 주의. O(V+E) 시간·저장 공간.

[[BFS]] · [[Backtracking]] · [[SCC]]
