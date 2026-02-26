---
type: algorithm
tags:
  - mst
  - spanning-tree
  - graph
  - union-find
language: java
status: in-review
source: []
---

# MST (Minimum Spanning Tree, 최소 신장 트리)

> **한 줄 요약:** 모든 정점을 잇는 트리 중 간선 가중치 합이 최소인 것

## Core Concept

- **When to use:** 연결 비용 최소화, 네트워크 설계, 무방향 가중치 그래프에서 트리 추출
- **Key Point:**
  - **무방향** 가중치 그래프에서 정의 (연결 그래프라고 가정)
  - 간선 n-1개로 정점 n개 연결, 사이클 없음
  - Prim(정점 확장)과 Kruskal(간선 정렬 + Union-Find) 두 가지 대표 알고리즘

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Add edge | `graph[u].add(new Edge(v,w)); graph[v].add(new Edge(u,w));` | 무방향 가중치 간선 |
| Select min | PQ에서 최소 비용 / 간선 배열 정렬 | Prim: 인접 최소 / Kruskal: 전역 최소 |
| Merge | `dsu.union(u, v)` | Kruskal에서 사이클 방지 |

## Patterns

### Pattern A: Prim (우선순위 큐, 정점 확장)

- 한 정점에서 시작해 현재 연결된 정점 집합에 인접한 간선 중 비용 최소인 것을 추가. PQ에 (정점, 비용) 넣고 확장.

```java
static class Edge {
    int to;
    long w;
    Edge(int to, long w) { this.to = to; this.w = w; }
}
static class Node {
    int v;
    long cost;
    Node(int v, long cost) { this.v = v; this.cost = cost; }
}

List<Edge>[] graph = new ArrayList[n+1];
for(int i=1;i<=n;i++) graph[i] = new ArrayList<>();
// ... 무방향 가중치 간선 추가 ...

boolean[] inMST = new boolean[n+1];
PriorityQueue<Node> pq = new PriorityQueue<>(Comparator.comparingLong(n -> n.cost));
pq.add(new Node(1, 0));
long total = 0;
int edges = 0;

while(!pq.isEmpty() && edges < n){
    Node node = pq.poll();
    int u = node.v;
    if(inMST[u]) continue;
    inMST[u] = true;
    total += node.cost;
    edges++;

    for(Edge e : graph[u]){
        if(!inMST[e.to]) pq.add(new Node(e.to, e.w));
    }
}
// edges == n 이면 MST 완성, total이 비용 합
```

### Pattern B: Kruskal (간선 정렬 + Union-Find)

- 간선을 가중치 오름차순 정렬 후, 사이클이 생기지 않으면 추가. Union-Find로 두 정점이 이미 같은 컴포넌트인지 확인.

```java
static class Edge implements Comparable<Edge> {
    int u, v;
    long w;
    Edge(int u, int v, long w) { this.u = u; this.v = v; this.w = w; }
    public int compareTo(Edge o) { return Long.compare(w, o.w); }
}

List<Edge> edges = new ArrayList<>();
// ... edges에 (u, v, w) 추가 (무방향이면 한 번만) ...

Collections.sort(edges);
DSU dsu = new DSU(n+1);  // 1..n
long total = 0;
int count = 0;

for(Edge e : edges){
    if(dsu.union(e.u, e.v)){
        total += e.w;
        if(++count == n-1) break;
    }
}
// count == n-1 이면 MST 완성
```

## Pitfalls & Tips

- 그래프가 연결이 아니면 MST 없음 (n-1개 간선을 못 채움)
- Prim은 시작 정점에 따라 트리 모양만 다를 뿐 비용 합은 동일
- Kruskal에서 DSU는 정점 번호(1-index)에 맞춰 크기 n+1로 생성
- 가중치 합이 int를 넘을 수 있으면 `long` 사용
