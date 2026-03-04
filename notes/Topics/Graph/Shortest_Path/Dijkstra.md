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
  - PQ에는 같은 정점이 여러 번 들어가므로 `if (d != dist[u]) continue;`로 stale 원소 제거
  - 시간복잡도: `O((V + E) log V)` (인접 리스트 + PQ)
  - B형에서는 정답 자체보다도, **자료형(long), 스킵 조건, 조기 종료**로 시간 차이가 큼

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Init dist | `Arrays.fill(dist, Long.MAX_VALUE); dist[start]=0;` | 시작점만 0, 나머지 무한 |
| Relax | `if(dist[v] > dist[u] + w){ dist[v]=dist[u]+w; pq.add(...); }` | 거리 갱신 |
| Lazy skip | `if(node.dist != dist[u]) continue;` | 오래된 PQ 원소 스킵 |
| Early stop | `if (u == target) break;` | 단일 도착점 문제에서 성능 개선 |

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
    if(node.dist != dist[u]) continue; // stale 원소 제거 (핵심 최적화)

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

### Pattern B: 단일 도착점이면 조기 종료
- `start -> target` 한 점만 필요할 때, target이 PQ에서 확정되는 순간 종료 가능
```java
while (!pq.isEmpty()) {
    Node cur = pq.poll();
    int u = cur.v;
    if (cur.dist != dist[u]) continue;
    if (u == target) break; // target 확정

    for (Edge e : graph[u]) {
        int v = e.to;
        long nd = dist[u] + e.w;
        if (nd < dist[v]) {
            dist[v] = nd;
            pq.add(new Node(v, nd));
        }
    }
}
```

### Pattern C: 간선 가중치가 0/1이면 0-1 BFS가 더 빠름
- 우선순위 큐 대신 `Deque` 사용 시 `O(V+E)` 가능
```java
Deque<Integer> dq = new ArrayDeque<>();
Arrays.fill(dist, INF);
dist[start] = 0;
dq.add(start);

while (!dq.isEmpty()) {
    int u = dq.pollFirst();
    for (Edge e : graph[u]) {
        int v = e.to;
        int nd = dist[u] + (int) e.w; // w는 0 또는 1
        if (nd >= dist[v]) continue;
        dist[v] = nd;
        if (e.w == 0) dq.addFirst(v); // 0비용은 앞쪽
        else          dq.addLast(v);  // 1비용은 뒤쪽
    }
}
```

## Pitfalls & Tips

- 음수 간선이 있으면 다익스트라 사용 불가 (음수 사이클 가능성)
- 거리 합이 int 범위를 넘을 수 있으므로 `long` 사용 권장
- PQ에 같은 정점이 여러 거리로 들어갈 수 있음 — 꺼낼 때 `dist[u]`와 비교해 스킵하는 방식이 일반적
- `visited[u]`로 한 번만 처리하는 구현도 가능하지만, 일반적으로 `dist` stale 검사 쪽이 안전하고 구현이 단순
- 정점 0/1-index와 `dist` 배열 크기 일치시키기
