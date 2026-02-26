---
type: algorithm
tags:
  - graph
  - shortest-path
  - dijkstra
language: java
status: in-review
source: []
---

# Dijkstra (다익스트라)

> **한 줄 요약:** 음수 간선이 없을 때 한 정점에서 나머지 정점까지의 최단거리를 구하는 알고리즘

## Core Concept

- **When to use:** 가중치 그래프에서 단일 시작점 최단경로 (음수 간선 없음)
- **Key Point:**
  - **음수 간선이 있으면 사용 불가** (벨만포드 등 다른 알고리즘 사용)
  - 매 단계에서 현재 거리가 가장 짧은 정점을 골라 relax (PQ 사용)
  - 한 번 확정된 정점은 다시 보지 않아도 됨 (PQ에서 꺼낸 뒤 dist 비교로 스킵)

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Init dist | `Arrays.fill(dist, Long.MAX_VALUE); dist[start]=0;` | 시작점만 0, 나머지 무한 |
| Relax | `if(dist[v] > dist[u] + w){ dist[v]=dist[u]+w; pq.add(...); }` | 거리 갱신 |
| Lazy skip | `if(node.dist != dist[u]) continue;` | 오래된 PQ 원소 스킵 |
| Visit | 방문 배열 대신 dist 비교로 중복 처리 | PQ 기반에서는 dist가 갱신된 것만 유효 |

## Patterns

### Pattern A: 단일 시작점 최단거리 (PQ + 인접 리스트)

```java
static class Edge {
    int to;
    long w;
    Edge(int to, long w) { this.to = to; this.w = w; }
}
static class Node {
    int v;
    long dist;
    Node(int v, long dist) { this.v = v; this.dist = dist; }
}

List<Edge>[] graph = new ArrayList[n+1];
for(int i=1;i<=n;i++) graph[i] = new ArrayList<>();
// ... graph에 간선 추가 (u -> v, w) ...

long[] dist = new long[n+1];
Arrays.fill(dist, Long.MAX_VALUE);
dist[start] = 0;

PriorityQueue<Node> pq = new PriorityQueue<>(Comparator.comparingLong(n -> n.dist));
pq.add(new Node(start, 0));

while(!pq.isEmpty()){
    Node node = pq.poll();
    int u = node.v;
    if(node.dist != dist[u]) continue;

    for(Edge e : graph[u]){
        int v = e.to;
        long w = e.w;
        if(dist[v] > dist[u] + w){
            dist[v] = dist[u] + w;
            pq.add(new Node(v, dist[v]));
        }
    }
}
```

## Pitfalls & Tips

- 음수 간선이 있으면 다익스트라 사용 불가 (음수 사이클 가능성)
- 거리 합이 int 범위를 넘을 수 있으므로 `long` 사용 권장
- PQ에 같은 정점이 여러 거리로 들어갈 수 있음 — 꺼낼 때 `dist[u]`와 비교해 스킵하는 방식이 일반적
- 정점 0/1-index와 `dist` 배열 크기 일치시키기
