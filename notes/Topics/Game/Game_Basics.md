---
type: algorithm
tags:
  - game
  - game-basics
language: java
status: in-review
source: []
---

# Game Basics (게임 문제 기본)
> **한 줄 요약:** "현재 상태가 이기는 상태인가?"를 정의하고 점화식으로 뒤에서부터 채우는 문제 유형

## Core Concept
* **When to use:**
  - 번갈아 수를 두는 2인 완전 정보 게임
  - 최선의 플레이를 가정했을 때 승패/점수를 구하는 문제
* **Key Point:**
  - 상태 `S`가 이기는 상태(win)인지 지는 상태(lose)인지 정의
  - 내 차례에서 **하나라도 lose 상태로 보낼 수 있으면 win**
  - 모든 다음 상태가 win이면 lose

## API & State Change (if applicable)
| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Transition | `nextStates(s)` | 상태 목록 | 가능한 다음 수 |
| Memo check | `if (memo[s] != -1)` | 값 반환 | 계산 캐시 재사용 |
| Win update | `if (!win[next]) win[s] = true;` | 상태 변경 | 역점화식 |

## Patterns
### Pattern A: 1차원 돌게임 DP (Take 1,3,4)
```java
// win[i] = 돌 i개에서 현재 플레이어가 이기면 true
boolean[] win = new boolean[N + 1];
int[] moves = {1, 3, 4};

for (int i = 1; i <= N; i++) {
    boolean canWin = false;
    for (int m : moves) {
        if (i - m < 0) continue;
        if (!win[i - m]) { // 상대를 패배 상태로 보낼 수 있으면 승리
            canWin = true;
            break;
        }
    }
    win[i] = canWin;
}
```

### Pattern B: DFS + Memo (상태 공간이 클 때)
```java
int[] memo = new int[MAX_STATE]; // -1: 미계산, 0: lose, 1: win
Arrays.fill(memo, -1);

int solve(int s) {
    if (isTerminal(s)) return 0; // 더 둘 수 없으면 패배
    if (memo[s] != -1) return memo[s];

    for (int ns : nextStates(s)) {
        if (solve(ns) == 0) return memo[s] = 1;
    }
    return memo[s] = 0;
}
```

### Pattern C: 점수 최대화 게임 (minimax)
```java
int f(int l, int r) {
    if (l > r) return 0;
    // 내가 왼쪽을 고르면 상대는 내 점수를 최소화하려고 함
    int takeL = a[l] - f(l + 1, r);
    int takeR = a[r] - f(l, r - 1);
    return Math.max(takeL, takeR);
}
```

## Pitfalls & Tips
- 턴 정보가 상태에 영향을 주면 `(state, turn)`까지 캐시 키에 포함
- "최대 점수" 문제를 "점수 차이"로 바꾸면 점화식이 단순해짐
- 상태 수가 많으면 대칭성 제거, 정렬된 상태화 등으로 상태 공간 축소 필요
