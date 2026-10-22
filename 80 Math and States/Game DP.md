---
type: template
aliases: ["Winning and Losing States", "Game_Basics"]
category: dynamic-programming
categories: [dynamic-programming]
---

# Game DP

```java
boolean[] buildWinningStates(int n, int[] moves) {
    // seed: 움직일 수 없는 0개 상태는 패배
    boolean[] win = new boolean[n + 1];
    // order: 제거 후의 작은 상태부터 계산
    for (int stones = 1; stones <= n; stones++) {
        for (int take : moves) {
            // win: 상대를 패배로 보내는 선택 하나면 승리
            if (take <= stones && !win[stones - take]) {
                win[stones] = true;
                break;
            }
        }
    }
    return win;
}
```

n≥0, moves는 양수 제거량 · 한 더미, 마지막 돌을 가져가면 승리 · 시간 O(n·moves.length), 공간 O(n).

**분류** [[00 Maps/Dynamic Programming|Dynamic Programming]]

**사용 원리** [[40 Optimization/DP|DP]]
