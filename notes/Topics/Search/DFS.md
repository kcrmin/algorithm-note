---
type: algorithm
tags:
  - search
  - dfs
language: java
status: in-review
source: []
---

# DFS (Depth-First Search)
> **한 줄 요약:** 한 경로를 끝까지 파고들며 탐색하는 기본 그래프/트리 탐색 기법

## Core Concept
* **When to use:**
  - 연결 요소 개수, 경로 존재 여부, 사이클 판별
  - 백트래킹/완전탐색의 기본 골격
* **Key Point:**
  - 재귀 또는 스택으로 구현 가능
  - 상태 방문 순서가 정답/성능에 영향을 줄 수 있음
  - B형에서는 `가지치기 + 방문 상태 표현 최적화`가 성능 핵심

## API & State Change

| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Visit | `visited[u] = true;` | 상태 변경 | 정점 방문 처리 |
| Recur | `dfs(v);` | 탐색 진행 | 인접 정점으로 이동 |
| Iterative push | `stack.push(v);` | 상태 변경 | 스택 기반 DFS |

## Patterns
### Pattern A: 재귀 DFS (인접 리스트)
- 코드가 짧아 구현 실수 줄이기 좋음
```java
List<Integer>[] g = new ArrayList[n];
boolean[] visited = new boolean[n];

void dfs(int u) {
    visited[u] = true; // 진입 즉시 방문 처리: 사이클/역간선에서 중복 재귀 방지
    for (int v : g[u]) {
        if (visited[v]) continue;
        dfs(v);
    }
}
```

### Pattern B: 반복형 DFS (스택)
- 재귀 깊이가 매우 큰 입력에서 스택 오버플로우 회피
```java
boolean[] visited = new boolean[n];
ArrayDeque<Integer> st = new ArrayDeque<>();
visited[start] = true; // push 시점 방문 처리로 동일 노드 중복 push 감소
st.push(start);

while (!st.isEmpty()) {
    int u = st.pop();
    for (int v : g[u]) {
        if (visited[v]) continue;
        visited[v] = true; // pop 시점이 아니라 push 시점에 체크해 상수 시간 최적화
        st.push(v);
    }
}
```

### Pattern C: DFS + Pruning (최적화 문제)
- 현재 해가 이미 최적해 이상이면 즉시 컷
```java
int best = Integer.MAX_VALUE;

void dfs(int depth, int cost) {
    if (cost >= best) return; // pruning
    if (depth == N) {
        best = Math.min(best, cost);
        return;
    }
    for (int cand : candidates[depth]) {
        dfs(depth + 1, cost + weight[cand]);
    }
}
```

## Pitfalls & Tips
- 방문 체크 시점이 늦으면 같은 노드가 중복 push되어 느려짐
- 그래프가 크면 재귀 DFS는 `StackOverflowError` 위험이 있으니 반복형도 준비
- 완전탐색 계열은 "분기 순서 최적화(좋은 후보 먼저)"가 pruning 효율을 크게 높임
