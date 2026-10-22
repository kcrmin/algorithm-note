---
type: template
tags: ["Search"]
aliases: ["DFS", "Depth-First Search"]
---

# DFS and Graphs

```java
// 무방향 graph는 List<Integer>[]이며 간선을 양쪽에 저장한다.
// visited는 처음에 모두 false.
static void dfs(int cur) {
    visited[cur] = true; // 들어오자마자 표시해야 0→1→0 순환을 멈춤
    for (int next : graph[cur]) {
        if (!visited[next]) dfs(next);
    }
}

static int countComponents(int n) {
    int count = 0;
    // 한 번의 DFS로 닿지 못하는 덩어리도 확인
    for (int start = 0; start < n; start++) {
        if (visited[start]) continue;
        count++; // 아직 미방문이면 새 연결 요소 하나의 시작
        dfs(start); // 같은 덩어리 전체를 표시해 이후 다시 세지 않음
    }
    return count;
}
```

**예시**

```text
0—1—2    3—4    5
DFS(0): {0,1,2}
DFS(3): {3,4}   DFS(5): {5}
새 DFS 시작 3번 = 연결 요소 3개
```

**주의** 최단거리 용도가 아니다. 일자 Graph는 StackOverflow에 주의. O(V+E) 시간·저장 공간.

[[BFS]] · [[Backtracking]] · [[SCC]]
