---
type: template
aliases: [Fenwick_Tree]
tags: [자료구조]
---

# Fenwick 트리

원소 하나에 값을 더하고, 구간 합을 빠르게 구한다.

> **기억:** 내부는 1부터: 더할 때 `i += i & -i`, 읽을 때 `i -= i & -i`; 구간 합은 접두 합 두 개의 차.

**전제:** 외부 0-index·양 끝 포함, 내부 1-index. 합은 long 범위. 갱신·질의 O(log N), 공간 O(N).

## 템플릿

`n`은 원소 수, `tree = new long[n + 1]`. 처음에는 0이며 `add(i, a[i])`로 채운다.

```java
void add(int index, long delta) {
    for (int i = index + 1; i <= n; i += i & -i) {
        tree[i] += delta;
    }
}
long prefixSum(int right) {
    long result = 0;
    for (int i = right + 1; i > 0; i -= i & -i) {
        result += tree[i];
    }
    return result;
}
long sum(int left, int right) {
    if (left > right) return 0;
    return prefixSum(right) - prefixSum(left - 1);
}
```

## 왜 이렇게 쓰나

- `tree[i]`는 끝이 i이고 길이가 `i & -i`인 구간 합이다. 조회는 방금 더한 구간 길이만큼 왼쪽으로 이동한다.
- `index + 1`로 시작해야 한다. 내부 0은 최하위 비트가 0이라 갱신이 진행되지 않는다.
- `add`는 대입이 아니다. 새 값으로 바꾸려면 `새 값 - 기존 값`을 더한다.

**추적:** `[2,1,3,4,5]`의 `prefixSum(4)`는 내부 `5→4→0`, 합은 `5+10=15`.

> [!info]- 더 자세한 설명
>
> > **핵심:** 내부 `tree[i]`는 끝점이 `i`이고 길이가 `i & -i`인 구간의 합이다. 외부 인덱스는 0부터, 내부 인덱스는 1부터 사용한다.
>
> ## 개념
>
> Fenwick 트리(Binary Indexed Tree, BIT)는 겹치는 몇 개의 구간 합을 저장해 **한 원소에 값 더하기와 구간 합**을 빠르게 처리한다. [[누적합]]은 원소 하나가 바뀌면 뒤쪽 누적합을 모두 고쳐야 하지만, Fenwick 트리는 그 원소를 포함하는 `O(log N)`개 구간만 고친다.
>
> ## 적용 조건
>
> | 연산 | 시간 |
> |---|---|
> | 아래 생성자 초기화 | `O(N log N)` |
> | `add(index, delta)` | `O(log N)` |
> | `prefixSum(right)`, `sum(left, right)` | `O(log N)` |
> | 공간 | `O(N)` |
>
> - 원소에 양수·음수를 더할 수 있고, 구간의 합을 묻는다. 이 템플릿은 일반적인 최솟값 질의용이 아니다.
> - 외부 API는 **0-index, 양 끝 포함**이다. `prefixSum(-1)`과 `left > right`인 빈 구간의 합은 `0`이다.
> - 값 자체를 대입하려면 기존 값을 따로 보관하고 `새 값 - 기존 값`을 더한다. [[좌표 압축]] 후 값별 등장 횟수를 관리할 때도 사용한다.
>
> `a = [2, 1, 3, 4, 5]`라면 내부 배열은 다음 구간을 저장한다. 내부 번호와 원본 번호를 섞지 않는 것이 핵심이다.
>
> | 내부 `i` | `i & -i` | 포함하는 원본 인덱스 | `tree[i]` |
> |---|---:|---|---:|
> | 1 | 1 | `[0, 0]` | 2 |
> | 2 | 2 | `[0, 1]` | 3 |
> | 3 | 1 | `[2, 2]` | 3 |
> | 4 | 4 | `[0, 3]` | 10 |
> | 5 | 1 | `[4, 4]` | 5 |
>
> `i & -i`는 이진수의 가장 오른쪽 `1`만 남긴다. 예를 들어 `6 = 110₂`, `6 & -6 = 010₂ = 2`이므로 내부 6번 노드는 내부 `[5, 6]`의 두 원소를 맡는다. 음수는 2의 보수이므로 이 연산으로 가장 낮은 비트 하나를 분리할 수 있다.
>
> - **조회는 담당 구간을 소비한다.** `prefixSum(4)`는 내부 `i = 5`에서 시작한다. `tree[5] = 5`를 더하고 `5 - 1 = 4`로 이동한 뒤 `tree[4] = 10`을 더한다. 다음은 `4 - 4 = 0`이므로 끝이며 결과는 `15`이다. 매번 방금 더한 구간의 길이를 빼므로 겹치거나 빠지는 원소가 없다.
> - **갱신은 포함하는 상위 구간으로 이동한다.** `add(2, 6)`은 내부 `3 → 4 → 8`로 이동한다. 유효한 3번·4번 노드만 `+6`이 되어 `tree[3] = 9`, `tree[4] = 16`이 된다. 가장 낮은 비트를 더하면 현재 구간을 포함하는 다음 Fenwick 구간으로 올라간다.
> - **구간 합은 두 접두 합의 차다.** 갱신 후 `sum(1, 3)`은 `prefixSum(3) - prefixSum(0) = 16 - 2 = 14`이고, 실제 원소도 `1 + 9 + 4 = 14`이다.
>
> ### 주의점
>
> - 내부 `i = 0`은 `i & -i = 0`이라 갱신 루프가 진행되지 않는다. 외부 `index + 1` 변환은 이 문제도 막는다.
> - `add`는 대입이 아니다. `add(2, 9)`는 기존 값에 9를 더한다.
> - 합과 변화량은 `long`이다. 모든 중간 합이 `long` 범위 안에 들어가야 한다.

> [!example]- 준비 코드 · 전체 구현
>
> ```java
> final class Fenwick {
>     private final int n;
>     private final long[] tree;
>
>     Fenwick(int n) {
>         if (n < 0) throw new IllegalArgumentException();
>         this.n = n;
>         tree = new long[n + 1];
>     }
>
>     Fenwick(long[] a) {
>         this(a.length);
>         for (int i = 0; i < n; i++) add(i, a[i]);
>     }
>
>     void add(int index, long delta) {
>         if (index < 0 || index >= n) throw new IndexOutOfBoundsException();
>         for (int i = index + 1; i <= n; i += i & -i) {
>             tree[i] += delta;
>         }
>     }
>
>     long prefixSum(int right) {
>         if (right < -1 || right >= n) throw new IndexOutOfBoundsException();
>         long result = 0;
>         for (int i = right + 1; i > 0; i -= i & -i) {
>             result += tree[i];
>         }
>         return result;
>     }
>
>     long sum(int left, int right) {
>         if (left > right) return 0;
>         if (left < 0 || right >= n) throw new IndexOutOfBoundsException();
>         return prefixSum(right) - prefixSum(left - 1);
>     }
> }
> ```
