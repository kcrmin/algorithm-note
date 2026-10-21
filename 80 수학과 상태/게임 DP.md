---
type: template
aliases: [Game_Basics]
tags: [DP]
---

# 게임 DP

현재 차례인 사람이 이길 수 있는 상태를 작은 상태부터 계산한다.

> **기억:** 상대를 패배 상태로 보내는 합법적인 수가 하나라도 있으면 승리; 없으면 패배.

**전제:** 한 더미에서 1·3·4개 제거, 마지막 돌을 가져가면 승리. N≥0, 시간 O(N×3), 공간 O(N).

## 템플릿

`win[stones]`는 남은 돌이 stones개일 때 현재 차례의 승패다. 상대 차례에도 같은 기준을 쓴다.

```java
boolean[] buildWinningStates(int n) {
    int[] moves = {1, 3, 4};
    boolean[] win = new boolean[n + 1];
    // win[0] = false: 가져갈 돌이 없으면 패배.
    for (int stones = 1; stones <= n; stones++) {
        for (int take : moves) {
            if (take <= stones && !win[stones - take]) {
                win[stones] = true;
                break;
            }
        }
    }
    return win;
}
```

## 왜 이렇게 쓰나

- 다음 상태는 상대 차례이므로 `!win[next]`를 찾는다. win[next]가 true인 수를 고르면 상대가 이긴다.
- `take <= stones`일 때만 합법이다. 모든 take가 양수여서 참조할 작은 상태는 이미 계산되어 있다.
- 이기는 수 하나면 충분해 break한다. 끝까지 없으면 초기값 false가 유지된다.

**추적:** `win[0]=false → win[1]=true → win[2]=false`; 7에서는 다음 6·4·3이 모두 true라 false.

> [!info]- 더 자세한 설명
>
> > **핵심:** 현재 차례의 플레이어가 상대를 패배 상태로 보낼 수 있는 수가 하나라도 있으면 승리 상태다. 모든 합법적인 수가 상대의 승리 상태로 이어지면 패배 상태다.
>
> ## 개념
>
> 게임 DP는 두 플레이어가 번갈아 최선의 선택을 하는 게임에서, 현재 상태의 승패를 더 작은 상태의 승패로 계산한다. `win[n]`은 특정 사람의 승패가 아니라 돌이 `n`개일 때 **현재 차례인 사람**이 이길 수 있는지를 뜻한다. 차례가 바뀔 때 관점도 바뀌므로 다음 상태가 패배 상태인 수를 찾는 것이 현재의 승리 조건이다.
>
> ## 적용 조건
>
> - 아래 규칙은 돌 더미 하나에서 차례마다 1개·3개·4개 중 하나를 가져가고, 가져갈 돌이 없는 사람이 지는 게임이다. 마지막 돌을 가져가는 사람이 이긴다.
> - 남은 돌 수만으로 다음 합법적인 수가 결정되고, 매 수마다 돌 수가 줄어드므로 작은 상태부터 [[DP]]를 계산할 수 있다.
> - 돌 수 상한이 `N`, 가능한 제거 개수가 `K`개라면 시간 `O(NK)`, 공간 `O(N)`이다. 아래 `K = 3`에서는 시간 `O(N)`이다.
> - 반복 가능한 상태나 무승부가 있는 게임은 이 단순한 배열 DP의 적용 대상이 아니다.
>
> 점화식은 `win[n] = OR(!win[n - take])`이며, `take ≤ n`인 합법적인 수에 대해서만 계산한다. 먼저 돌이 0개이면 움직일 수 없어 `win[0] = false`다.
>
> | 돌 수 `n` | 가능한 다음 상태 | `win[n]` | 판정 이유 |
> |---|---|---|---|
> | 0 | 없음 | 패배 | 합법적인 수 없음 |
> | 1 | 0 | 승리 | 상대에게 패배 상태 0을 줌 |
> | 2 | 1 | 패배 | 유일한 다음 상태 1이 상대의 승리 상태 |
> | 3 | 2, 0 | 승리 | 1개 또는 3개를 가져가 패배 상태로 보냄 |
> | 4 | 3, 1, 0 | 승리 | 4개를 가져가 0으로 보냄 |
> | 5 | 4, 2, 1 | 승리 | 3개를 가져가 2로 보냄 |
> | 6 | 5, 3, 2 | 승리 | 4개를 가져가 2로 보냄 |
> | 7 | 6, 4, 3 | 패배 | 세 다음 상태가 모두 상대의 승리 상태 |
> | 8 | 7, 5, 4 | 승리 | 1개를 가져가 7로 보냄 |
>
> `!win[stones - take]`의 부정이 중요하다. 돌을 가져간 다음에는 상대 차례이므로 그 배열 값이 `false`여야 내가 이긴다. 이기는 수 하나만 있으면 충분해 `break`할 수 있고, 그런 수가 끝까지 없으면 초기값 `false`가 유지된다.
>
> 바깥 반복에서 `stones`를 계산하기 전에는 `0..stones-1`의 승패가 모두 확정되어 있다. 모든 `take`가 양수이므로 참조하는 `stones - take`는 항상 이미 계산된 상태다. 이 불변식과 `win[0]`에서 출발하는 귀납으로 각 상태의 승패가 확정된다.
>
> ### 주의점
>
> - `chooseWinningMove`에 넘기는 배열은 같은 이동 규칙으로 `buildWinningStates`가 계산한 것이어야 한다. `win[8]`만 확인하면 승패, `chooseWinningMove(8, win)`은 이기는 선택인 1을 준다.
> - 마지막 돌을 가져가는 사람이 지는 규칙이면 종료 상태와 마지막 수 처리부터 다시 정해야 한다. 이 코드에서 출력만 뒤집어 해결할 수 없다.
> - 이전 선택이나 차례에 따라 가능한 수가 달라지면 그 정보도 상태에 포함한다. 독립된 여러 더미의 조합은 단순히 각 더미의 승패를 OR로 합칠 수 없다.

> [!example]- 준비 코드 · 전체 구현
>
> ```java
> public final class GameDP {
>     private static final int[] MOVES = {1, 3, 4};
>
>     public static boolean[] buildWinningStates(int maxStones) {
>         if (maxStones < 0) {
>             throw new IllegalArgumentException("stones must be nonnegative");
>         }
>         boolean[] win = new boolean[maxStones + 1];
>         // win[0] = false: 현재 차례에 가져갈 돌이 없으면 패배.
>
>         for (int stones = 1; stones <= maxStones; stones++) {
>             for (int take : MOVES) {
>                 if (take <= stones && !win[stones - take]) {
>                     win[stones] = true;
>                     break;
>                 }
>             }
>         }
>         return win;
>     }
>
>     // 이미 계산한 win 배열에서 이기는 수 하나를 반환한다.
>     // 패배 상태 또는 돌이 0개인 상태이면 -1을 반환한다.
>     public static int chooseWinningMove(int stones, boolean[] win) {
>         if (stones < 0 || stones >= win.length) {
>             throw new IllegalArgumentException("state is outside win array");
>         }
>         for (int take : MOVES) {
>             if (take <= stones && !win[stones - take]) {
>                 return take;
>             }
>         }
>         return -1;
>     }
> }
> ```
