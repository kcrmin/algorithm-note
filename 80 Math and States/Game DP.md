---
type: template
aliases: ["Winning and Losing States", "Game_Basics"]
category: dynamic-programming
categories: [dynamic-programming]
---

# Game DP

```java
boolean[] buildWinningStates(int n, int[] moves) {
    boolean[] win = new boolean[n + 1]; // win[0]=false: 움직이지 못하면 패배.
    for (int stones = 1; stones <= n; stones++) {
        for (int take : moves) {
            // 상대를 패배 상태에 보내는 수가 하나라도 있으면 현재는 승리다.
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
