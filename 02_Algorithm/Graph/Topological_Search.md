---
type: algorithm
tags:
  - graph
  - topological-sort
language: java
status: in-review
source: []
---

# Topological Sort (위상정렬)

> **한 줄 요약:** DAG에서 선후관계를 만족하는 정점의 순서를 얻는 알고리즘

## Core Concept

- **When to use:** 선후관계·의존성 순서, 작업 스케줄링, DAG에서의 DP 순서
- **Key Point:**
  - **DAG(Directed Acyclic Graph)** 에서만 정의됨 — 사이클이 있으면 위상정렬 불가
  - u → v 간선이 있으면 정렬 결과에서 u가 v보다 앞에 와야 함
  - 유일하지 않음 (같은 DAG에서도 여러 위상순서가 존재할 수 있음)
  - Kahn(BFS)과 DFS 두 가지 구현이 있음

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Indegree 0 | `indeg[v] == 0` | 선행 정점이 모두 처리된 정점 |
| Decrease | `indeg[v]--;` | 간선 제거 효과 (Kahn) |
| Order | `order.add(u);` | 결과 순서에 추가 |
| Cycle | `order.size() < n` | 사이클 존재 시 일부 정점 미포함 |

## Patterns

### Pattern A: Kahn (BFS, indegree 기반)

- 진입차수 0인 정점부터 제거하며 순서를 만듦. 구현이 직관적이고 사이클 검사가 쉬움.

```java
int n = sc.nextInt();
int m = sc.nextInt();

List<Integer>[] graph = new ArrayList[n+1];
int[] indeg = new int[n+1];
for(int i=1;i<=n;i++) graph[i] = new ArrayList<>();

for(int i=0;i<m;i++){
    int u = sc.nextInt();
    int v = sc.nextInt();
    graph[u].add(v);
    indeg[v]++;
}

Queue<Integer> q = new ArrayDeque<>();
List<Integer> order = new ArrayList<>();

for(int i=1;i<=n;i++){
    if(indeg[i] == 0) q.add(i);
}

while(!q.isEmpty()){
    int u = q.poll();
    order.add(u);

    for(int v : graph[u]){
        indeg[v]--;
        if(indeg[v] == 0) q.add(v);
    }
}

// order.size() < n 이면 사이클 존재 (위상정렬 불가)
```

### Pattern B: DFS (post-order 후 역순)

- DFS로 끝나는 순서를 기록한 뒤 뒤집으면 위상순서. 재귀 한 번으로 처리할 때 유리.

```java
List<Integer>[] graph = new ArrayList[n+1];
for(int i=1;i<=n;i++) graph[i] = new ArrayList<>();
// ... 입력으로 graph, indeg 구성 생략 ...

boolean[] visited = new boolean[n+1];
List<Integer> order = new ArrayList<>();

for(int i=1;i<=n;i++){
    if(!visited[i]) dfs(i);
}

Collections.reverse(order);
// order가 위상순서

void dfs(int u){
    visited[u] = true;
    for(int v : graph[u]){
        if(!visited[v]) dfs(v);
    }
    order.add(u);  // post-order: 자식 처리 후 추가
}
```

## Pitfalls & Tips

- 위상정렬 결과 개수가 n보다 작으면 **사이클 존재** — DAG가 아님
- 여러 연결 요소가 있어도 indegree 0인 정점은 모두 큐에 넣어야 함 (Kahn)
- DFS 방식은 사이클 검사가 별도 필요 (회색/검정 방문으로 back edge 검사)
- 정점 번호 0/1-index와 order 사용 방식 일치시키기
