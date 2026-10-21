---
type: template
tags: [자료구조]
---

# Sparse Table

> **핵심:** `table[k][i]`는 `i`부터 길이 `2^k`인 구간의 최솟값이다. 질의 구간을 덮는 두 블록이 겹쳐도 최솟값은 바뀌지 않는다.

## 개념

Sparse Table은 값이 바뀌지 않는 배열에서 길이가 2의 거듭제곱인 모든 구간의 답을 미리 저장한다. 최솟값처럼 같은 값을 여러 번 합쳐도 결과가 같은 연산은, 질의 구간을 두 블록으로 덮어 `O(1)`에 답할 수 있다. 아래 코드는 정적 구간 최솟값(RMQ, Range Minimum Query)을 처리한다.

## 적용 조건

| 연산 | 시간 |
|---|---|
| 전처리 | `O(N log N)` |
| `min(left, right)` | `O(1)` |
| 공간 | `O(N log N)` |

- **갱신이 없고 구간 최솟값 질의가 많을 때** 사용한다. 갱신이 필요하면 [[세그먼트 트리]]가 맞는다.
- 두 블록을 겹쳐 답하려면 결합법칙과 멱등성(`op(x, x) = x`)이 필요하다. `min`, `max`, `gcd`, 비트 AND·OR는 가능하지만 합·XOR는 이 `O(1)` 공식으로 처리할 수 없다.
- 외부 API는 **0-index, 양 끝 포함**이다. 빈 구간의 최솟값은 항등원 `Long.MAX_VALUE`로 정의한다.

## Java 템플릿

```java
final class SparseTable {
    private final int n;
    private final int[] log;
    private final long[][] table;

    SparseTable(long[] a) {
        n = a.length;
        log = new int[n + 1];
        for (int i = 2; i <= n; i++) log[i] = log[i / 2] + 1;

        int levels = n == 0 ? 0 : log[n] + 1;
        table = new long[levels][n];
        if (n == 0) return;
        System.arraycopy(a, 0, table[0], 0, n);

        for (int k = 1; k < levels; k++) {
            int length = 1 << k;
            int half = length >> 1;
            for (int i = 0; i <= n - length; i++) {
                table[k][i] = Math.min(table[k - 1][i],
                                       table[k - 1][i + half]);
            }
        }
    }

    long min(int left, int right) {
        if (left > right) return Long.MAX_VALUE;
        if (left < 0 || right >= n) throw new IndexOutOfBoundsException();
        int k = log[right - left + 1];
        return Math.min(table[k][left], table[k][right - (1 << k) + 1]);
    }
}
```

> [!info]- 원리와 코드 설명
>
> `a = [5, 2, 6, 1, 4]`일 때 표에서 실제로 사용하는 값은 다음과 같다. 배열 범위를 넘는 시작점은 계산하거나 조회하지 않는다.
>
> | `k` | 블록 길이 | `table[k]`의 유효한 값 |
> |---|---:|---|
> | 0 | 1 | `[5, 2, 6, 1, 4]` |
> | 1 | 2 | `[2, 2, 1, 1]` |
> | 2 | 4 | `[1, 1]` |
>
> - **큰 블록은 두 절반을 합친다.** `table[2][0]`은 길이 4인 `[0, 3]`이다. 길이 2인 `[0, 1]`과 `[2, 3]`의 최솟값을 합쳐 `min(2, 1) = 1`을 얻는다. 오른쪽 절반의 시작점이 `i + half`인 이유다.
> - **로그는 버림값이다.** `log[1] = 0`, `log[2] = 1`, `log[3] = 1`, `log[4] = 2`이다. 양의 길이를 2로 나누면 가장 큰 2의 거듭제곱 지수도 하나 줄어들어 `log[i] = log[i / 2] + 1`이 된다. 부동소수점 로그의 반올림 문제가 없다.
> - **질의는 왼쪽 블록과 오른쪽 블록을 겹친다.** `min(1, 3)`의 길이는 3이므로 `k = 1`, 블록 길이는 2다. 왼쪽 `[1, 2]`와 오른쪽 `[2, 3]`을 골라 `min(2, 1) = 1`을 반환한다. 오른쪽 끝을 `right`에 맞추므로 시작점은 `right - 2^k + 1`이다.
> - **빈틈이 없고 겹침은 허용된다.** `2^k ≤ 길이 < 2^(k+1)`이므로 두 블록의 길이 합이 질의 길이 이상이다. 가운데 값 6을 두 번 고려해도 `min(6, 6) = 6`이므로 답은 그대로다. 합이라면 6이 중복 계산되므로 같은 공식을 쓸 수 없다.
>
> ### 주의점
>
> - 원본 배열을 나중에 바꿔도 이미 만든 표는 바뀌지 않는다. 새 값 기준으로는 다시 만들어야 한다.
> - `log[0]`은 질의에 사용하지 않는다. 빈 구간을 먼저 처리하므로 빈 배열에서도 안전하다.
> - 이 코드는 합의 `0` 대신 최솟값의 항등원 `Long.MAX_VALUE`를 반환한다. 빈 구간이 허용되지 않는 문제라면 호출 범위를 먼저 맞춘다.
> - `O(N log N)` 메모리를 사용한다. `long` 표 본체만 대략 `8 × N × (⌊log₂N⌋ + 1)`바이트이며 Java 배열 객체 비용은 별도다.
