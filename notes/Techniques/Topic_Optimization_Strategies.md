---
type: technique
tags:
  - technique
  - topic-optimization-strategies
language: java
status: in-review
source: []
---

# Topic Optimization Strategies
> **한 줄 요약:** 자주 출제되는 주제별로 "정답 코드"를 "통과 코드"로 바꾸는 최적화 포인트 정리

## Core Concept
* **When to use:**
  - 주제별로 무엇을 최적화해야 하는지 빠르게 복습할 때
  - 시험 직전 체크리스트를 만들 때
* **Key Point:**
  - 동일한 Big-O라도 구현 상수 차이가 크게 난다
  - 주제별 병목을 미리 알면 디버깅 시간을 줄일 수 있다

## API & State Change (if applicable)
| Topic | Default | Optimization Pivot | 설명 |
| :--- | :--- | :--- | :--- |
| Dijkstra/Heap | `PQ<Node>` | stale skip, 조기 종료, 객체 최소화 | 최단거리/이벤트 처리 |
| Bucket/Hash | `HashMap` | 배열 버킷, 좌표 압축 | 키 조회 상수 시간 절감 |
| Union-Find | 재귀 find | size/rank + 경로 압축 + 반복형 find | 연결성 쿼리 최적화 |
| DFS/Game | 완전탐색 | pruning + 메모이제이션 | 상태 공간 축소 |
| Segment Tree | 재귀 구현 | iterative 또는 lazy 정확화 | 로그 쿼리 안정화 |
| Trie | 객체 노드 | 배열 풀(node pool) | GC/메모리 최적화 |

## Patterns
### Pattern A: 60% 빈출 축(다익스트라, 힙, 버킷, 유니온파인드)
```java
// Dijkstra
if (curDist != dist[u]) continue; // stale skip

// Bucket
freq[value]++; // 해시 대신 배열

// Union-Find
if (size[ra] < size[rb]) { int t = ra; ra = rb; rb = t; }
parent[rb] = ra;
```

### Pattern B: 40% 축(DFS, SegmentTree, Trie, Game, Hash)
```java
// DFS/Game
if (cost >= best) return; // pruning

// SegmentTree
if (l <= s && e <= r) return tree[node]; // 완전 포함

// Trie
if (nxt[cur][c] == 0) nxt[cur][c] = nodes++;
```

### Pattern C: 공통 성능 루틴
```java
// 1. long 사용으로 overflow 예방
long nd = dist[u] + w;

// 2. 배열 재사용
Arrays.fill(dist, INF);

// 3. 반복문 내부 new 최소화
// (객체 생성 대신 primitive/배열 선호)
```

## Pitfalls & Tips
- 출제 비중이 높은 주제부터 "실수 없이 10분 내 구현" 가능한 템플릿을 고정
- 자료구조를 바꿀 수 있는지 먼저 확인하고, 그 다음 미세 최적화
- 테스트케이스 수가 많으면 초기화 비용도 총 시간에 크게 반영됨
