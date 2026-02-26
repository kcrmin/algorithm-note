---
type: technique
tags:
  - dist
  - shortest-path
language: java
status: in-review
source: []
---

# Dist 정리

## Dist란?
Dist는 **시작점에서 각 상태까지 도달하는 데 필요한 최소 비용**을 저장해, 이미 더 적은 비용으로 방문한 상태는 다시 탐색하지 않도록 하는 배열이다.
주로 최단 거리·최소 횟수 문제에서 사용된다.

```java
dist[next] = dist[cur] + 1;
```

## 핵심 규칙
- **BFS에서만 최단 거리 보장**
- DFS + dist는 거의 항상 오답

## BFS에서의 사용 패턴
```java
Arrays.fill(dist, -1);  // dist 초기값을 -1로 두면 visited를 대체 가능
dist[start] = 0;
q.add(start);

while (!q.isEmpty()) {
    int cur = q.poll();
    for (int next : adj[cur]) {
        if (dist[next] != -1) continue;
        dist[next] = dist[cur] + 1;
        q.add(next);
    }
}
```

## 주의사항
- dist 초기값을 -1로 두면 visited를 대체 가능
- "최단"이 없으면 dist는 필요 없다

## 요약
Dist는 최소 몇 단계로 도달했는지를 기록한다.
