---
type: concept
tags:
  - graph
  - directed-graph
language: java
status: in-review
source: []
---

# Directed Graph (유향 그래프)

> **한 줄 요약:** 간선에 방향이 있어 (u → v)와 (v → u)를 다르게 취급하는 그래프

## Core Concept

- **When to use:** 선후관계, 의존성, 위상정렬, SCC
- **Key Point:**
  - 간선은 한쪽 방향만 저장: `graph[u].add(v)`만 수행
  - in-degree/out-degree가 의미가 있음
  - 위상정렬은 `Topological_Search.md` 참고

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Init | `List<Integer>[] graph = new ArrayList[n+1];` | 그래프 생성 |
| Add Edge | `graph[u].add(v);` | 유향 간선 추가 |
| In-degree | `indeg[v]++;` | 진입차수 증가 |
| Iterate | `for(int v : graph[u]){}` | 인접 정점 순회 |
| Visit | `visited[v] = true;` | 방문 체크 |

## Patterns

### Pattern A: 입력 저장 (u v) + indegree 구성

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
```

## Pitfalls

- 정점 번호 0/1-index 확인
- 유향 그래프에서는 간선을 한 방향만 저장 (무방향처럼 양쪽 추가하면 안 됨)