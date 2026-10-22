---
type: template
tags: ["Graphs"]
aliases: ["Strongly Connected Components", "Kosaraju"]
---

# SCC

**언제** Directed Graph에서 서로 왕복 가능한 정점들을 하나의 묶음으로 압축한다.

```text
원본  0 ↔ 1 → 2        종료 순서 [2,1,0]
역방향에서 종료 역순 0,1,2로 시작
0에서 {0,1} 묶음 → 2는 따로 묶음
압축  {0,1} → {2}      결과는 DAG
```

**코드** `graph/reverse`는 원본/역방향 인접 리스트. visited=false, component=-1, finishOrder는 빈 목록.

```java
static void finish(int cur) {
    visited[cur] = true;
    for (int next : graph[cur]) {
        if (!visited[next]) finish(next);
    }
    finishOrder.add(cur); // ①
}
static void assign(int cur, int id) {
    component[cur] = id;
    for (int next : reverse[cur]) {
        if (component[next] == -1) assign(next, id);
    }
}
static int decompose(int n) {
    for (int v = 0; v < n; v++) {
        if (!visited[v]) finish(v);
    }
    int count = 0;
    for (int i = finishOrder.size() - 1; i >= 0; i--) { // ②
        int start = finishOrder.get(i);
        if (component[start] == -1) assign(start, count++);
    }
    return count;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 방문 순서가 아닌 종료 순서 | 서로 다른 SCC에서 A→B이면 A의 가장 늦은 종료가 B보다 늦다. |
| ② 종료 역순 + 역방향 | A부터 꺼내도 뒤집은 간선으로는 다음 SCC B로 흘러가지 않는다. |
| 두 번째는 component로 방문 확인 | 첫 DFS의 visited는 이미 전부 true다. |

**주의** Kosaraju 방식. 긴 일자 Graph는 재귀 StackOverflow에 주의. O(V+E) 시간·공간.

[[DFS and Graphs]] · [[Topological Sort]]
