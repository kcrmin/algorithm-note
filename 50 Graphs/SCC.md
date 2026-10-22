---
type: template
tags: ["Graphs"]
aliases: ["Strongly Connected Components", "Kosaraju"]
category: connectivity-ordering
categories: [connectivity-ordering]
---

# SCC

```java
static List<Integer>[] graph, reverse;
static boolean[] visited;
static int[] component;
static List<Integer> finishOrder;

// finish: 원본 DFS의 종료 순서 기록
static void finish(int cur) {
    visited[cur] = true;
    for (int next : graph[cur]) {
        if (!visited[next]) finish(next);
    }
    // postorder: 자식을 모두 탐색한 뒤 현재 정점 기록
    finishOrder.add(cur);
}
// assign: 역방향 DFS 한 번으로 SCC 하나 배정
static void assign(int cur, int id) {
    component[cur] = id;
    for (int next : reverse[cur]) {
        if (component[next] == -1) assign(next, id);
    }
}
// decompose: 종료 역순으로 미배정 SCC 분리
static int decompose(int n) {
    for (int v = 0; v < n; v++) {
        if (!visited[v]) finish(v);
    }
    int count = 0;
    for (int i = finishOrder.size() - 1; i >= 0; i--) {
        int start = finishOrder.get(i);
        if (component[start] == -1) assign(start, count++);
    }
    return count;
}
```

원본/역방향 `List<Integer>[] graph, reverse`; `visited=false`, `component=-1`, 빈 `finishOrder`에서 `decompose(n)`→SCC 수; 깊은 재귀 주의. O(V+E).

**분류** [[00 Maps/Connectivity and Ordering|Connectivity and Ordering]]

**사용 원리** [[30 Search/DFS and Graphs|DFS and Graphs]]
