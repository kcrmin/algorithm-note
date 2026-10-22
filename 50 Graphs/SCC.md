---
type: template
tags: ["Graphs"]
aliases: ["Strongly Connected Components", "Kosaraju"]
---

# SCC

```java
// graph/reverse는 원본/역방향 List<Integer>[]. visited=false, component=-1로 준비.
// finishOrder는 빈 List<Integer>로 시작한다.
// 두 번째 탐색은 visited 대신 component==-1로 미방문을 판정한다.
static void finish(int cur) {
    visited[cur] = true;
    for (int next : graph[cur]) {
        if (!visited[next]) finish(next);
    }
    // 자식 탐색이 끝난 뒤 기록: SCC 사이 A→B면 A가 더 늦게 끝남
    finishOrder.add(cur);
}
static void assign(int cur, int id) {
    component[cur] = id;
    // 간선을 뒤집어 아직 안 묶인 다음 SCC로 흘러가지 않게 함
    for (int next : reverse[cur]) {
        if (component[next] == -1) assign(next, id);
    }
}
static int decompose(int n) {
    for (int v = 0; v < n; v++) {
        if (!visited[v]) finish(v);
    }
    int count = 0;
    // 가장 늦게 끝난 SCC부터 역방향 탐색
    for (int i = finishOrder.size() - 1; i >= 0; i--) {
        int start = finishOrder.get(i);
        // 한 번의 assign이 왕복 가능한 한 묶음 전체를 번호화
        if (component[start] == -1) assign(start, count++);
    }
    return count;
}
```

**예시**

```text
원본  0 ↔ 1 → 2        종료 순서 [2,1,0]
역방향에서 종료 역순 0,1,2로 시작
0에서 {0,1} 묶음 → 2는 따로 묶음
압축  {0,1} → {2}      결과는 DAG
```

**주의** Kosaraju 방식. 긴 일자 Graph는 재귀 StackOverflow에 주의. O(V+E) 시간·공간.

[[DFS and Graphs]] · [[Topological Sort]]
