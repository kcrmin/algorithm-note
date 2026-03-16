---
type: technique
tags:
  - technique
  - distance
language: java
status: in-review
source: []
---

# Dist 정리

> **한 줄 요약:** dist 배열은 "현재까지 알고 있는 최소 비용"을 저장해 중복 탐색을 줄이고 최단거리 계산을 가능하게 한다

## Core Concept
* **When to use:**
  - 최소 이동 횟수/최소 비용/최단거리 문제
  - 같은 상태를 여러 경로로 방문할 수 있는 그래프/격자 문제
* **Key Point:**
  - 무가중치 그래프: BFS로 최초 방문 거리가 최단거리
  - 양의 가중치 그래프: Dijkstra에서 `dist`를 기준으로 relax
  - `dist`는 visited의 상위 개념 (더 좋은 비용이 나오면 재방문 가능)

## API & State Change
| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Init | `Arrays.fill(dist, INF); dist[s]=0;` | 초기화 | 시작점 거리 0 |
| Relax | `if (nd < dist[v]) dist[v]=nd;` | 상태 갱신 | 더 짧은 경로 발견 |
| Skip | `if (curDist != dist[u]) continue;` | 분기 | 오래된 상태 제거 |

## Patterns
### Pattern A: 무가중치 그래프 BFS 거리
```java
int[] dist = new int[n];
Arrays.fill(dist, -1); // -1: 미방문

ArrayDeque<Integer> q = new ArrayDeque<>();
dist[start] = 0;
q.add(start);

while (!q.isEmpty()) {
    int u = q.poll();
    for (int v : g[u]) {
        if (dist[v] != -1) continue;
        dist[v] = dist[u] + 1;
        q.add(v);
    }
}
```

### Pattern B: 가중치 그래프 Dijkstra 거리
```java
long[] dist = new long[n];
Arrays.fill(dist, INF);
dist[start] = 0;

PriorityQueue<Node> pq = new PriorityQueue<>(Comparator.comparingLong(a -> a.d));
pq.add(new Node(start, 0));

while (!pq.isEmpty()) {
    Node cur = pq.poll();
    if (cur.d != dist[cur.v]) continue; // stale skip

    for (Edge e : g[cur.v]) {
        long nd = cur.d + e.w;
        if (nd >= dist[e.to]) continue;
        dist[e.to] = nd;
        pq.add(new Node(e.to, nd));
    }
}
```

## Pitfalls & Tips
- BFS 최단거리는 간선 가중치가 모두 동일(보통 1)일 때만 성립
- INF는 덧셈 시 overflow가 안 나도록 `Long.MAX_VALUE` 대신 충분히 큰 상수 사용 권장
- dist 배열 재사용 시 이전 테스트케이스 값 초기화를 빼먹지 않기
