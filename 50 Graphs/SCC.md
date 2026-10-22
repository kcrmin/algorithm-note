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

static void finish(int cur) {
    visited[cur] = true;
    for (int next : graph[cur]) {
        if (!visited[next]) finish(next);
    }
    finishOrder.add(cur); // 종료 후 기록: SCC 사이 A→B면 A가 더 늦게 끝남
}
static void assign(int cur, int id) {
    component[cur] = id;
    for (int next : reverse[cur]) { // 간선을 뒤집어 아직 안 묶인 다음 SCC로 못 넘어가게 함
        if (component[next] == -1) assign(next, id);
    }
}
static int decompose(int n) {
    for (int v = 0; v < n; v++) {
        if (!visited[v]) finish(v);
    }
    int count = 0;
    for (int i = finishOrder.size() - 1; i >= 0; i--) { // 가장 늦게 끝난 SCC부터 묶음
        int start = finishOrder.get(i);
        if (component[start] == -1) assign(start, count++);
    }
    return count;
}
```

원본/역방향 `List<Integer>[] graph, reverse`; `visited=false`, `component=-1`, 빈 `finishOrder`에서 `decompose(n)`→SCC 수; 깊은 재귀 주의. O(V+E).

**분류** [[00 Maps/Connectivity and Ordering|Connectivity and Ordering]]

**사용 원리** [[30 Search/DFS and Graphs|DFS and Graphs]]
