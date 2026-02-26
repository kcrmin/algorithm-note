---
type: algorithm
tags:
  - graph
  - shortest-path
  - bellman-ford
language: java
status: in-review
source: []
---

# Bellman-Ford (벨만-포드)

> **한 줄 요약:** 음수 간선이 있어도 단일 시작점 최단경로를 구할 수 있는 알고리즘 (음수 사이클 검사 가능)

## Core Concept

- **When to use:** 음수 간선이 있는 그래프, 음수 사이클 존재 여부 판별
- **Key Point:**
  - 간선을 **V-1번** 라운드만큼 전부 relax (V = 정점 수)
  - 한 라운드마다 모든 간선 (u,v,w)에 대해 `dist[v] = min(dist[v], dist[u]+w)`
  - V번째 라운드에서도 갱신되면 **음수 사이클** 존재
  - 시간 복잡도 O(VE)

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Init | `dist[s]=0`, 나머지 무한 | 시작점만 0 |
| Relax | `if(dist[u]!=INF && dist[v]>dist[u]+w) dist[v]=dist[u]+w` | 한 간선 relax |
| Round | V-1번 전체 간선 순회 | 최단거리 수렴 |
| Cycle | V번째에 갱신되면 음수 사이클 | 검사 |

## Patterns

### Pattern A: 단일 시작점 + 음수 사이클 검사

```java
long[] dist = new long[n+1];
Arrays.fill(dist, Long.MAX_VALUE);
dist[start] = 0;

for (int round = 0; round < n - 1; round++) {
    for (int u = 1; u <= n; u++) {
        if (dist[u] == Long.MAX_VALUE) continue;
        for (Edge e : graph[u]) {
            int v = e.to;
            long w = e.w;
            if (dist[v] > dist[u] + w)
                dist[v] = dist[u] + w;
        }
    }
}

boolean hasNegCycle = false;
for (int u = 1; u <= n; u++) {
    if (dist[u] == Long.MAX_VALUE) continue;
    for (Edge e : graph[u]) {
        if (dist[e.to] > dist[u] + e.w) {
            hasNegCycle = true;
            break;
        }
    }
}
```

## Pitfalls & Tips

- 음수 사이클이 있으면 해당 컴포넌트에서 도달 가능한 정점의 최단거리는 의미 없음 (무한히 작아짐)
- 간선 리스트로 저장해 두고 매 라운드마다 순회하는 구현이 편함
- dist가 INF인 정점은 relax에서 제외해도 됨 (갱신 의미 없음)
- long 사용 권장 (underflow/overflow)
