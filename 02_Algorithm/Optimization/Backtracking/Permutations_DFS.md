---
type: algorithm
tags:
  - permutations
  - backtracking
language: java
status: in-review
source: []
---

# Permutations (DFS)
> **한 줄 요약:** 순열은 "순서를 고려"한다 (중복 순열은 used 없이, 순열은 used로 중복 선택을 막는다)

## Core Concept
* **When to use:**
  - 순열 (nPr) / 중복 순열(n^r)을 모두 생성해야 할 때
* **Key Point:**
  - 순열(중복 X): 같은 원소를 다시 쓰면 안 됨 → `used[]` 사용
  - 중복 순열(중복 O): 같은 원소 재사용 가능 → `used[]` 없음

## Patterns
### Pattern A: 순열 nPr (중복 X)
- N개 중 R개를 나열, 순서 고려, 같은 원소 재사용 불가
```java
int[] pick = new int[R];
boolean[] used = new boolean[N];

void dfs(int depth) {
    if (depth == R) {
        // 순열 완성
        return;
    }

    for (int i = 0; i < N; i++) {
        if (used[i]) continue;

        used[i] = true;
        pick[depth] = i;     // i 선택
        dfs(depth + 1);
        used[i] = false;     // 되돌리기
    }
}
```

### Pattern B: 중복 순열 (중복 O)
- N개 중 R개를 나열, 순서 고려, 같은 원소 재사용 가능
```java
int[] pick = new int[R];

void dfs(int depth) {
    if (depth == R) {
        // 중복 순열 완성
        return;
    }

    for (int i = 0; i < N; i++) {
        pick[depth] = i;     // i 선택 (재사용 가능)
        dfs(depth + 1);
    }
}
```

## Pitfalls & Tips
- 순열에서 used 복구를 빼먹으면 결과가 깨진다
- 중복 순열에 used를 쓰면 "순열"이 되어버린다
- 입력 값 자체에 중복이 있는 경우(예: [1,1,2])는 "같은 값 스킵" 로직이 추가로 필요할 수 있음
