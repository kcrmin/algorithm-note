---
type: technique
tags:
  - visited
  - graph
language: java
status: in-review
source: []
---

# Visited 정리
> **한 줄 요약:** 같은 상태를 다시 처리하지 않기 위한 표시

## Visited란?
Visited는 **이미 처리한 상태를 다시 방문하지 않도록 막는 장치**다.  
주 목적은 **무한 루프 방지와 중복 탐색 제거**다.

## 언제 쓰나
- 그래프에 사이클이 있을 때
- 격자에서 재방문을 막아야 할 때
- 같은 상태를 여러 번 처리하면 안 되는 문제

```java
if (visited[next]) continue;
visited[next] = true;
```

## 안 써도 되는 경우
- 트리 탐색 (부모 → 자식 구조)
- 백트래킹 (상태를 되돌림)
- 깊이가 단조 증가하는 DFS

## 주의사항
- BFS/DFS에서 **방문 체크 시점**이 중요
- BFS에서는 보통 **큐에 넣을 때** 방문 처리

## 요약
Visited는 다시 보면 안 되는 상태를 막기 위한 표시다.
