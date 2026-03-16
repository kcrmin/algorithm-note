---
type: technique
tags:
  - technique
  - visited
language: java
status: in-review
source: []
---

# Visited 정리
> **한 줄 요약:** 같은 상태를 다시 처리하지 않기 위한 표시

## Core Concept
* **When to use:**
  - 그래프/격자에 사이클이 있어 무한 루프 가능성이 있을 때
  - 같은 상태를 중복 처리하면 시간 초과가 나는 문제
* **Key Point:**
  - visited는 정답을 바꾸는 로직이 아니라, **중복 탐색 제거** 로직
  - BFS는 보통 `큐에 넣을 때` 방문 처리, DFS는 진입 시점 방문 처리
  - 테스트케이스가 많으면 visited 초기화 방식(`fill` vs timestamp)도 성능 요소

## API & State Change

| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Check | `if (visited[v]) continue;` | 분기 | 이미 방문한 상태 스킵 |
| Mark | `visited[v] = true;` | 상태 변경 | 방문 표시 |
| Reset | `Arrays.fill(visited, false);` | 초기화 | 다음 테스트케이스 준비 |

## Patterns
### Pattern A: BFS 방문 처리
```java
ArrayDeque<Integer> q = new ArrayDeque<>();
boolean[] visited = new boolean[n];

visited[start] = true; // enqueue 시점에 표시
q.add(start);

while (!q.isEmpty()) {
    int u = q.poll();
    for (int v : g[u]) {
        if (visited[v]) continue;
        visited[v] = true;
        q.add(v);
    }
}
```

### Pattern B: timestamp visited (초기화 비용 최적화)
- `T`가 큰 문제에서 `Arrays.fill` 반복 비용을 줄일 수 있음
```java
int[] seen = new int[n];
int stamp = 1;

// 테스트케이스 시작 시 stamp++만 수행
if (seen[v] == stamp) {
    // 이미 방문
} else {
    seen[v] = stamp; // 방문 표시
}
```

## Pitfalls & Tips
- BFS에서 dequeue 시점 방문 처리하면 중복 enqueue가 늘어 느려질 수 있음
- 트리처럼 사이클이 없으면 부모 정보만으로 visited를 생략할 수 있음
- 상태가 `(x, y, dir)`처럼 복합이면 visited 차원도 상태 정의에 맞춰야 함
