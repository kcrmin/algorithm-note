---
type: algorithm
tags:
  - combinations
  - backtracking
language: java
status: in-review
source: []
---

# Combinations (DFS)
> **한 줄 요약:** 조합은 "인덱스를 앞으로만 진행"해서 중복을 막는다 (중복 조합은 start를 유지)

## Core Concept
* **When to use:**
  - nCr / nHr(중복 조합)을 모두 생성해야 할 때
* **Key Point:**
  - 조합(중복 X): 다음 선택은 항상 더 뒤에서 시작 → `dfs(i + 1, depth + 1)`
  - 중복 조합(중복 O): 같은 원소를 다시 선택 가능 → `dfs(i, depth + 1)`

## Patterns
### Pattern A: 조합 nCr (중복 X)
- N개 중 R개 선택, 순서 무시, 같은 원소 재사용 불가
```java
// 선택된 인덱스를 pick[0..R-1]에 저장한다고 가정
void dfs(int start, int depth) {
    if (depth == R) {
        // 조합 완성
        return;
    }

    for (int i = start; i < N; i++) {
        pick[depth] = i;          // i 선택
        dfs(i + 1, depth + 1);    // 다음은 i보다 뒤에서만 선택
    }
}
```

### Pattern B: 중복 조합 nHr (중복 O)
- N개 중 R개 선택, 순서 무시, 같은 원소 재사용 가능
```java
void dfs(int start, int depth) {
    if (depth == R) {
        // 중복 조합 완성
        return;
    }

    for (int i = start; i < N; i++) {
        pick[depth] = i;        // i 선택
        dfs(i, depth + 1);      // i를 다시 선택할 수 있으므로 start = i 유지
    }
}
```

## Pitfalls & Tips
- nCr에서 `dfs(i, ...)`로 가면 중복 조합이 된다
- nHr에서 `dfs(i + 1, ...)`로 가면 중복이 사라져 조합이 된다
- 입력 값 자체에 중복이 있는 경우(예: [1,1,2])는 "중복 제거(스킵)" 로직이 추가로 필요할 수 있음
