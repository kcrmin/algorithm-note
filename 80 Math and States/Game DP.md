---
type: template
aliases: ["Winning and Losing States", "Game_Basics"]
---

# Game DP

**언제** 두 사람이 번갈아 움직이고, 현재 상태의 승패가 다음 상태의 승패로 결정될 때.

```text
규칙: 한 더미에서 1·3·4개 제거, 마지막 돌을 가져가면 승리
남은 돌     0  1  2  3  4  5  6  7
현재 승패   L  W  L  W  W  W  W  L
7의 다음 상태: 6(W), 4(W), 3(W) → 전부 상대 승리 → L
```

**코드** `win[s]` = 돌 s개에서 현재 차례인 사람이 이기는가.

```java
boolean[] buildWinningStates(int n) {
    int[] moves = {1, 3, 4};
    boolean[] win = new boolean[n + 1]; // win[0] = false
    for (int stones = 1; stones <= n; stones++) {
        for (int take : moves) {
            if (take <= stones && !win[stones - take]) { // ①
                win[stones] = true;
                break;                                 // ②
            }
        }
    }
    return win;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 다음 상태가 패배면 승리 | 다음 차례는 상대이므로 상대가 지는 상태로 보내면 된다. |
| ② 이기는 수 하나면 충분 | 최선의 수를 고르므로 다른 수가 나빠도 상관없다. |
| 0개는 패배 | 더 가져갈 돌이 없어 움직일 수 없다. |

**주의** 마지막 돌을 가져가면 지는 규칙은 초기 상태부터 달라진다. O(n·이동 종류) 시간, O(n) 공간.

[[DP]] · [[Bitmask]]
