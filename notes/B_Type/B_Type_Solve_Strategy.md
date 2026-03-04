---
type: technique
tags:
  - b-type
  - b-type-solve-strategy
language: java
status: in-review
source:
  - "Dijkstra"
  - "DFS"
  - "DP_Basics"
  - "Backtracking"
  - "[[B_Type_Optimization_Playbook]]"
---

# B형 Solve Strategy

> **한 줄 요약:** 정답 아이디어보다 먼저 `통과 가능한 구현 전략`을 고정하는 절차.

## 0. 30초 복잡도 예산

- `N`, `M`, 상태 수를 보고 허용 가능한 상한을 먼저 계산
- `O(N^2)`가 가능한지, `O(N log N)`가 필요한지 즉시 결정
- 모호하면 보수적으로 한 단계 빠른 모델 선택

## 1. 3줄 모델링

- 상태: `dist[u]`, `dp[i][j]`, `visited[x][y]` 중 무엇인지
- 전이: 어떤 연산이 상태를 갱신하는지
- 자료구조: 배열/PQ/Deque/Hash 중 선택 근거

## 2. 먼저 버릴 것 결정

- 전수 탐색이면 pruning 기준부터 정의
- 그래프면 중복 방문/stale 상태 제거 조건부터 정의
- 매 테스트케이스 재할당 대신 배열 재사용 가능 여부 확인

## 3. 구현 순서

1. 입력 + 자료구조 선언
2. 초기화(base, INF, start)
3. 핵심 루프(전이)
4. 예외 처리(불가능/음수 사이클/도달 불가)

## 4. 제출 직전 1분 점검

- int overflow 가능성 -> long 승격
- 재귀 깊이 위험 -> iterative 대체
- `Scanner/Stream/boxing` 과다 사용 여부 확인

## 연결 노트

- 최단경로: Dijkstra, Bellman_Ford, Floyd_Warshall
- 탐색/가지치기: DFS, Pruning, Backtracking
- 상태 관리: Visited, Distance, Memoization
