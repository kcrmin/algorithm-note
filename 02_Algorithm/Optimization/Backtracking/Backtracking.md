---
type: technique
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

## Pitfalls & Tips
- 되돌리기(unchoose)를 빼먹으면 상태가 오염됨
- 가지치기 조건을 넣으면 성능이 크게 개선됨
- DFS 구조가 보이면 백트래킹을 의심
