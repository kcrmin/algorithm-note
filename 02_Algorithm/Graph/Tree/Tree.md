---
type: concept
tags:
  - tree
  - graph
language: java
status: in-review
source: []
---

# Tree (트리)

> **한 줄 요약:** 사이클이 없는 연결 그래프 (정점 n개면 간선 n-1개)

## Core Concept

- **When to use:** 계층구조, 서브트리, LCA, 트리 DP
- **Key Point:**
  - 입력은 보통 무방향 간선으로 주어지고, 루트를 잡아 parent/child로 처리
  - 트리는 임의의 두 정점 사이 경로가 유일
  - 루트가 없으면 보통 1번을 루트로 잡음

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Init | `List<Integer>[] graph = new ArrayList[n+1];` | 그래프 생성 |
| Add Edge | `graph[u].add(v); graph[v].add(u);` | 트리 간선 추가 |
| Rooting | `dfs(root, 0);` | parent/depth 채움 |
| Parent | `parent[v] = cur;` | 부모 저장 |
| Depth | `depth[v] = depth[cur] + 1;` | 깊이 저장 |

## Patterns

### Pattern A: 입력 저장 (n-1줄)

```java
int n = sc.nextInt();

List<Integer>[] graph = new ArrayList[n+1];
for(int i=1;i<=n;i++) graph[i] = new ArrayList<>();

for(int i=0;i<n-1;i++){
    int u = sc.nextInt();
    int v = sc.nextInt();
    graph[u].add(v);
    graph[v].add(u);
}
```

### Pattern B: 루트 트리 만들기 (parent 전달 DFS)

```java
int[] parent = new int[n+1];
int[] depth = new int[n+1];

dfs(1, 0); // root=1

void dfs(int cur, int p){
    parent[cur] = p;

    for(int v : graph[cur]){
        if(v == p) continue;
        depth[v] = depth[cur] + 1;
        dfs(v, cur);
    }
}
```

## Pitfalls

- 트리는 visited 없이 parent로 되돌아가는 간선을 막는 방식이 깔끔함
- 루트가 문제에서 주어지는지 확인 (없으면 1 또는 임의 노드)
- 재귀 DFS는 n이 크면 스택 오버플로 가능 (필요하면 iterative로 변경)