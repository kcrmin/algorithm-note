---
type: technique
tags:
  - b-type
  - b-type-performance-debug-checklist
language: java
status: in-review
source:
  - "[[B_Type_Optimization_Playbook]]"
  - "Distance"
  - "Visited"
  - "Prefix_Sum"
  - "Bitmask"
---

# B형 Performance Debug Checklist

> **한 줄 요약:** 정답은 맞는데 느릴 때, 원인을 빠르게 좁히기 위한 체크리스트.

## TLE 체크

- `Scanner`, `String.split`, Stream API 사용 여부
- 동일 상태 중복 방문 여부 (`visited`, `stale skip`)
- 자료구조 미스매치 (Map -> 배열/압축 가능?)
- 불필요 정렬/복사/객체 생성 반복 여부

## MLE 체크

- 테스트케이스마다 대형 배열 재할당 여부
- `List<List<...>>` 중첩 객체 과다 여부
- 상태를 더 작은 타입/압축 표현으로 바꿀 수 있는지

## WA + 성능 동시 체크

- INF overflow (`dist[u] + w`)
- 0/1-index 혼합
- pruning 조건이 정답을 깨지 않는지

## 실전 절차

1. 가장 바깥 루프 반복 횟수부터 계측
2. hottest loop에서 분기/조회/객체 생성 수 확인
3. 배열화, 조기 종료, 상태 압축 순서로 적용
4. 변경 후 동일 테스트로 재계측

## 연결 노트

- 최단경로: Dijkstra, Bellman_Ford
- 탐색 최적화: DFS, Pruning, Backtracking
- 자료구조 전환: Hash, Bucket, Fenwick_Tree
