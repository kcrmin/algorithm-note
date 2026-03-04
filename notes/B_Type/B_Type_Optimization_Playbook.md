---
type: technique
tags:
  - b-type
  - b-type-optimization-playbook
language: java
status: in-review
source:
  - "[[B_Type_Master_Index]]"
  - "[[B_Type_Performance_Debug_Checklist]]"
---

# Samsung B형 Optimization Playbook
> **한 줄 요약:** 정답 아이디어를 코드로 옮길 때, 시간/메모리 병목을 사전에 차단하기 위한 실전 체크리스트

## Linked Notes

- [[B_Type_Master_Index]]
- [[B_Type_Solve_Strategy]]
- [[B_Type_Implementation_Kit]]
- [[B_Type_Performance_Debug_Checklist]]
- [[B_Type_Mistake_Log_Template]]

## Core Concept
* **When to use:**
  - 구현은 맞는데 시간초과/메모리초과가 반복될 때
  - "아이디어는 맞는데 B형에서 느린 코드"를 줄여야 할 때
* **Key Point:**
  - 문제를 풀기 전에 `복잡도 예산`부터 계산
  - 병목은 보통 `입출력`, `자료구조 선택`, `불필요한 중복 계산`에서 발생
  - 최적화는 "작은 테크닉"보다 "잘못 고른 모델/알고리즘"을 먼저 고치는 순서로 진행

## API & State Change (if applicable)
| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Budget check | `ops = N * logN;` | 추정치 | 연산량 사전 계산 |
| Skip stale | `if (cur != dist[u]) continue;` | 분기 | 중복 상태 제거 |
| Prune | `if (cost >= best) return;` | 분기 | 탐색량 축소 |
| Reuse buffer | `Arrays.fill(arr, 0);` | 상태 재사용 | 테스트케이스 최적화 |

## Patterns
### Pattern A: 30초 복잡도 예산 체크
```java
// 대략적인 통과 기준(자바 기준 경험칙)
// 1e7 ~ 3e7 연산: 비교적 안전
// 1e8 이상: 자료구조/상수 시간 최적화 필요

long ops1 = (long) n * n;               // O(N^2)
long ops2 = (long) m * 20;              // O(M log N), log2(1e6) ≈ 20
long ops3 = (long) states * branching;  // DFS 분기 수
```

### Pattern B: 자료구조 선택 우선순위
```java
// 1) 범위 작음 + 정수 키 -> int[] bucket
// 2) 순서 통계/구간 쿼리 -> Fenwick/Segment Tree
// 3) 최단경로(음수 X) -> Dijkstra + PQ
// 4) 연결성 쿼리 -> Union-Find
// 5) 접두사 -> Trie
```

### Pattern C: DFS/백트래킹 최적화 루틴
```java
void dfs(int depth, int score) {
    if (score >= best) return; // pruning 1
    if (depth == N) {
        best = Math.min(best, score);
        return;
    }

    // 좋은 후보 먼저 시도 -> best를 빨리 갱신 -> pruning 강화
    for (int cand : ordered[depth]) {
        if (!valid(cand)) continue; // pruning 2
        choose(cand);
        dfs(depth + 1, score + delta(cand));
        undo(cand);
    }
}
```

## Pitfalls & Tips
- 같은 결과를 내는 더 단순한 모델(예: PQ 대신 deque, map 대신 배열)이 없는지 먼저 확인
- "정답은 맞는데 느린 코드"의 대부분은 중복 연산과 객체 생성 과다에서 발생
- 최적화는 항상 계측과 함께: 느린 부분을 정하고 그 부분만 바꾸기
