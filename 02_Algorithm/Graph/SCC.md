---
type: algorithm
tags:
  - graph
  - scc
  - strongly-connected-components
language: java
status: in-review
source: []
---

# SCC (Strongly Connected Components, 강한 연결 요소)

> **한 줄 요약:** 유향 그래프에서 서로 도달 가능한 정점들의 최대 집합

## Core Concept

- **When to use:** 유향 그래프의 묶음 분석, 2-SAT, DAG로의 압축(컴포넌트 DAG)
- **Key Point:**
  - **강하게 연결**: 같은 SCC 안의 임의의 두 정점 u, v에 대해 u→v, v→u 경로가 모두 존재
  - SCC끼리 묶으면 항상 **DAG**가 됨 (사이클은 한 컴포넌트 안에만)
  - Kosaraju(역방향 그래프 + 두 번 DFS) 또는 Tarjan(한 번 DFS, low link)으로 구함

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Reverse | `rev[v].add(u)` when `graph[u].add(v)` | 역방향 그래프 |
| Finish order | DFS 후 정점을 끝나는 순서로 기록 | Kosaraju 1차 DFS |
| Component id | `sccId[v] = id` | 정점이 속한 SCC 번호 |

## Patterns

### Pattern A: Kosaraju (역방향 그래프 + 두 번 DFS)

- 1) 원래 그래프로 DFS, 끝나는 순서를 스택/리스트에 기록. 2) 역방향 그래프에서 끝나는 순서의 역순으로 DFS, 한 번에 도달하는 것이 하나의 SCC.

```java
List<Integer>[] graph = new ArrayList[n+1];
List<Integer>[] rev = new ArrayList[n+1];
for(int i=1;i<=n;i++){
    graph[i] = new ArrayList<>();
    rev[i] = new ArrayList<>();
}
// graph[u].add(v); 할 때 rev[v].add(u); 도 수행

boolean[] visited = new boolean[n+1];
List<Integer> order = new ArrayList<>();

for(int i=1;i<=n;i++){
    if(!visited[i]) dfs1(i);
}
void dfs1(int u){
    visited[u] = true;
    for(int v : graph[u]){
        if(!visited[v]) dfs1(v);
    }
    order.add(u);
}

int[] sccId = new int[n+1];
int id = 0;
Arrays.fill(visited, false);
for(int i = order.size()-1; i >= 0; i--){
    int u = order.get(i);
    if(!visited[u]){
        id++;
        dfs2(u, id);
    }
}
void dfs2(int u, int id){
    visited[u] = true;
    sccId[u] = id;
    for(int v : rev[u]){
        if(!visited[v]) dfs2(v, id);
    }
}
// sccId[v]가 같은 것이 같은 SCC
```

### Pattern B: Tarjan (한 번 DFS, low link)

- DFS 스택과 `discovered` 순서, `low`(도달 가능한 최소 discovered)를 유지. `u`에서 부모로 돌아가는 간선을 제외하고, `low[u] == discovered[u]`이면 `u`가 SCC의 루트 — 스택에서 `u`까지 pop한 것이 한 SCC.

```java
List<Integer>[] graph = new ArrayList[n+1];
// ... 그래프 구성 ...

int[] discovered = new int[n+1];
int[] low = new int[n+1];
int[] sccId = new int[n+1];
boolean[] onStack = new boolean[n+1];
Deque<Integer> stack = new ArrayDeque<>();
int timer = 0;
int id = 0;

for(int i=1;i<=n;i++){
    if(discovered[i] == 0) tarjan(i);
}
void tarjan(int u){
    discovered[u] = low[u] = ++timer;
    stack.push(u);
    onStack[u] = true;

    for(int v : graph[u]){
        if(discovered[v] == 0){
            tarjan(v);
            low[u] = Math.min(low[u], low[v]);
        } else if(onStack[v]){
            low[u] = Math.min(low[u], discovered[v]);
        }
    }

    if(low[u] == discovered[u]){
        id++;
        while(true){
            int x = stack.pop();
            onStack[x] = false;
            sccId[x] = id;
            if(x == u) break;
        }
    }
}
```

## Pitfalls & Tips

- Kosaraju: 역방향 그래프를 반드시 만들어 두고, 1차 DFS 끝나는 **역순**으로 2차 DFS
- Tarjan: `onStack` 체크 없이 `discovered`만 쓰면 다른 SCC를 잘못 묶을 수 있음
- 정점 0/1-index와 배열 크기 일치
- SCC 개수는 `id`, 같은 `sccId`끼리 묶어서 사용
