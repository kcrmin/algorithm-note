---
type: concept
tags:
  - backtracking
language: java
status: in-review
source: []
---

# Backtracking
> **한 줄 요약:** 선택 → 재귀 → 되돌리기를 통해 가능한 모든 경우를 탐색하는 기본 DFS 기법

## Core Concept
* **When to use:**
  - 모든 경우를 탐색해야 하는 문제
  - 선택의 순서/조합이 결과에 영향을 줄 때
* **Key Point:**
  - 선택 (choose)
  - 재귀 호출 (explore)
  - 되돌리기 (un-choose)
  - B형 핵심은 "완전탐색을 하되, 유망하지 않은 분기는 최대한 빨리 자르기"

## Pattern: 기본 Backtracking 템플릿
```java
void dfs(int depth) {
    if (depth == 목표) {
        // 정답 처리
        return;
    }

    for (각 선택지) {
        choose();        // 선택
        dfs(depth + 1);  // 다음 단계
        unchoose();      // 되돌리기
    }
}
```

## Pattern: 가지치기 + 분기 순서 최적화
```java
int best = Integer.MAX_VALUE;

void dfs(int depth, int cost) {
    if (cost >= best) return;         // pruning 1: 현재가 이미 최적해 이상
    if (depth == N) {
        best = Math.min(best, cost);  // 정답 갱신
        return;
    }

    // 분기 순서를 "좋은 후보 먼저"로 두면 best를 더 빨리 갱신 가능
    for (int cand : orderedCandidates[depth]) {
        if (!canUse(cand)) continue;
        choose(cand);
        dfs(depth + 1, cost + delta(cand));
        unchoose(cand);
    }
}
```

## Pitfalls & Tips
- 되돌리기(unchoose)를 빼먹으면 상태가 오염됨
- 가지치기 조건을 넣으면 성능이 크게 개선됨
- 전역 배열/버퍼를 재사용하고, `new`를 재귀 안에서 만들지 않기
- DFS 구조가 보이면 백트래킹을 의심
