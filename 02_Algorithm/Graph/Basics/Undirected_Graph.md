---
type: concept
tags:
  - graph
  - undirected-graph
language: java
status: in-review
source: []
---

# Undirected Graph (무방향 그래프)

> **한 줄 요약:** 간선에 방향이 없어 (u, v)와 (v, u)를 동일하게 취급하는 그래프

## Core Concept

- **When to use:** 양방향관계, 연결성, 탐색, MST
- **Key Point:**
  - 간선 (u, v)는 자동으로 (v, u) 의미 포함
  - 인접 리스트에 양쪽 모두 추가해야 함
  - 연결 요소, 트리/포리스트 문제에서 자주 등장

## API & State Change

| Operation | Code                                          | 설명             |
| :-------- | :-------------------------------------------- | :--------------- |
| Init      | `List<Integer>[] graph = new ArrayList[n+1];` | 그래프 생성      |
| Add Edge  | `graph[u].add(v); graph[v].add(u);`           | 무방향 간선 추가 |
| Iterate   | `for(int v : graph[u]){}`                 | 인접 정점 순회   |
| Visit     | `visited[v] = true;`                          | 방문 체크        |

## Patterns

### Pattern A: 입력 저장 (u v)

```java
int n = sc.nextInt();
int m = sc.nextInt();

List<Integer>[] graph = new ArrayList[n+1];
for(int i=1;i<=n;i++) graph[i] = new ArrayList<>();

for(int i=0;i<m;i++){
    int u = sc.nextInt();
    int v = sc.nextInt();
    graph[u].add(v);
    graph[v].add(u);
}
```

### Pattern B: 연결 요소 (Connected Component) 개수 세기

```java
boolean[] visited = new boolean[n+1];
int components = 0;

for(int i=1;i<=n;i++){
    if(visited[i]) continue;
    components++;
    dfs(i);
}

void dfs(int u){
    visited[u] = true;
    for(int v : graph[u]){
        if(!visited[v]) dfs(v);
    }
}
```

## Pitfalls

- 정점 번호 0/1-index 확인
- 간선 수 m이 0일 수도 있음
- 무방향 그래프에서는 간선 하나가 리스트에 2번 저장됨
