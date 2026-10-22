---
type: template
tags: ["Search"]
aliases: ["DFS", "Depth-First Search"]
category: traversal-trees
categories: [traversal-trees]
---

# DFS and Graphs

```java
static List<Integer>[] graph;
static boolean[] visited;

static void dfs(int cur) { // dfs: 한 연결 요소의 정점 모두 방문
    // mark: 다시 들어오지 않도록 먼저 방문 표시
    visited[cur] = true;
    for (int next : graph[cur]) {
        if (!visited[next]) dfs(next);
    }
}
// countComponents: 새 DFS를 시작한 횟수 세기
static int countComponents(int n) {
    int count = 0;
    for (int start = 0; start < n; start++) {
        if (visited[start]) continue;
        count++;
        dfs(start);
    }
    return count;
}
```

양방향 `List<Integer>[] graph`, `visited=new boolean[n]`; `countComponents(n)`은 연결 요소 수. 깊은 경로는 재귀 스택 주의. O(V+E).

**분류** [[00 Maps/Traversal and Trees|Traversal and Trees]]
