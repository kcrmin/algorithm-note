---
type: template
aliases: ["Winning and Losing States", "Game_Basics"]
category: dynamic-programming
categories: [dynamic-programming]
---

# Game DP

```text
가져갈 수 있는 돌: 1개 또는 3개
남은 돌       0  1  2  3  4
현재 차례     L  W  L  W  L

돌 3개 ── 3개 가져감 ──▶ 돌 0개 (상대 L) → 나는 W
돌 4개 ─┬ 1개 가져감 ──▶ 돌 3개 (상대 W)
        └ 3개 가져감 ──▶ 돌 1개 (상대 W) → 나는 L

W: 상대를 L로 보내는 선택이 하나라도 있음
```

| Step | Pseudocode |
| --- | --- |
| Base | // 돌이 없으면 움직일 수 없으므로 현재 차례의 패배로 둔다. |
| Order | // 작은 돌 개수부터 계산해 이동할 상태의 승패를 먼저 알아둔다. |
| Choose | // 가능한 이동 중 상대가 패배하는 상태가 하나라도 있으면 승리다. |
| Keep losing | // 모든 이동이 상대의 승리로 이어지거나 이동이 없으면 패배다. |

## Java

```java
boolean[] buildWinningStates(int n, int[] moves) {
    boolean[] win = new boolean[n + 1];

    for (int stones = 1; stones <= n; stones++) {
        for (int take : moves) {
            // 이동 뒤의 상태는 상대 차례이므로 승패를 뒤집어 본다.
            if (take <= stones && !win[stones - take]) {
                win[stones] = true;
                break;
            }
        }
    }
    return win;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `win[stones]`의 주체 | 항상 현재 차례의 사람이다. 이동 후에는 상대 차례이므로 `!win[...]`을 찾는다. |
| 하나라도 / 전부 | 패배 상태로 가는 선택 하나면 승리. 패배는 모든 선택이 상대 승리일 때다. |
| `win[0] = false` | 마지막 돌을 가져가는 사람이 이기는 규칙. 마지막 돌을 가져가면 지는 게임에는 이 초기값을 그대로 쓰지 않는다. |
| 양수 이동량 | moves는 양수만 허용. 그래야 항상 이미 계산한 작은 상태로 이동한다. |

n≥0 · 한 더미 제거 게임 · 반환 배열에서 win[n]이 시작자의 승리 여부 · 시간 O(n·moves.length), 공간 O(n).

**분류** [[00 Maps/Dynamic Programming|Dynamic Programming]]

**사용 원리** [[40 Optimization/DP|DP]]
