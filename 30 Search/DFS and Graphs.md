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

static void dfs(int cur) {
    visited[cur] = true; // 재방문 전에 표시해 순환을 끊음
    for (int next : graph[cur]) {
        if (!visited[next]) dfs(next);
    }
}
static int countComponents(int n) {
    int count = 0;
    for (int start = 0; start < n; start++) {
        if (visited[start]) continue;
        count++; // 이전 DFS가 못 닿은 새 연결 요소
        dfs(start);
    }
    return count;
}
```

양방향 `List<Integer>[] graph`, `visited=new boolean[n]`; `countComponents(n)`은 연결 요소 수. 깊은 경로는 재귀 스택 주의. O(V+E).

**분류** [[00 Maps/Traversal and Trees|Traversal and Trees]]
