---
type: template
aliases: ["Winning and Losing States", "Game_Basics"]
---

# Game DP

```java
// 한 더미에서 1·3·4개 제거. 마지막 돌을 가져가면 승리하는 규칙.
boolean[] buildWinningStates(int n) {
    int[] moves = {1, 3, 4};
    // win[s]=현재 차례의 승패. win[0]=false: 움직일 수 없어 패배.
    boolean[] win = new boolean[n + 1];
    // 다음 상태는 더 작으므로 이미 계산되어 있다.
    for (int stones = 1; stones <= n; stones++) {
        for (int take : moves) {
            // 합법적인 수로 상대를 패배 상태에 보내면 승리.
            if (take <= stones && !win[stones - take]) {
                win[stones] = true;
                // 이기는 수가 하나만 있어도 된다. 없으면 초기 false가 유지된다.
                break;
            }
        }
    }
    return win;
}
```

**예시**

```text
규칙: 한 더미에서 1·3·4개 제거, 마지막 돌을 가져가면 승리
남은 돌     0  1  2  3  4  5  6  7
현재 승패   L  W  L  W  W  W  W  L
7의 다음 상태: 6(W), 4(W), 3(W) → 전부 상대 승리 → L
```

**주의** 마지막 돌을 가져가면 지는 규칙은 초기 상태부터 달라진다. O(n·이동 종류) 시간, O(n) 공간.

[[DP]] · [[Bitmask]]
