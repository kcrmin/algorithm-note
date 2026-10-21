---
type: template
tags: [자료구조]
---

# Lazy Propagation

> **핵심:** `tree[node]`의 구간 합에는 갱신이 이미 반영되어 있다. `lazy[node]`는 자식 구간에 아직 전달하지 않은 원소당 추가량이다.

## 개념

Lazy Propagation은 [[세그먼트 트리]]에서 구간 전체를 바꿀 때, 그 구간을 대표하는 노드만 갱신하고 자식의 갱신을 미루는 방법이다. 질의나 갱신 때문에 자식으로 내려갈 때만 미뤄 둔 값을 전달하므로 원소를 하나씩 바꾸지 않는다. 아래 코드는 **구간에 같은 값 더하기 + 구간 합**을 처리한다.

## 적용 조건

| 연산 | 시간 |
|---|---|
| 생성 | `O(N)` |
| `add(left, right, delta)` | `O(log N)` |
| `sum(left, right)` | `O(log N)` |
| 공간 | `O(N)` |

- 외부 API는 **0-index, 양 끝 포함**이다. `left > right`이면 갱신은 아무 일도 하지 않고 합은 `0`이다.
- 구간 길이와 추가량만으로 새 구간 합을 계산할 수 있다. 원소마다 `delta`를 더하면 구간 합에는 `delta × 구간 길이`가 더해진다.
- 추가량끼리는 합쳐서 미룰 수 있다. 구간 대입·곱하기를 처리하려면 지연 태그와 합성 규칙을 별도로 설계해야 한다.

## Java 템플릿

```java
final class LazySegmentTree {
    private final int n;
    private final long[] tree, lazy;

    LazySegmentTree(long[] a) {
        n = a.length;
        tree = new long[Math.max(1, 4 * n)];
        lazy = new long[tree.length];
        if (n > 0) build(1, 0, n - 1, a);
    }

    private void build(int node, int lo, int hi, long[] a) {
        if (lo == hi) { tree[node] = a[lo]; return; }
        int mid = (lo + hi) >>> 1;
        build(node * 2, lo, mid, a);
        build(node * 2 + 1, mid + 1, hi, a);
        tree[node] = tree[node * 2] + tree[node * 2 + 1];
    }

    private void apply(int node, int lo, int hi, long delta) {
        tree[node] += delta * (hi - lo + 1);
        if (lo != hi) lazy[node] += delta;
    }

    private void push(int node, int lo, int hi) {
        if (lazy[node] == 0 || lo == hi) return;
        int mid = (lo + hi) >>> 1;
        apply(node * 2, lo, mid, lazy[node]);
        apply(node * 2 + 1, mid + 1, hi, lazy[node]);
        lazy[node] = 0;
    }

    void add(int left, int right, long delta) {
        if (left > right) return;
        checkRange(left, right);
        add(1, 0, n - 1, left, right, delta);
    }

    private void add(int node, int lo, int hi, int l, int r, long delta) {
        if (r < lo || hi < l) return;
        if (l <= lo && hi <= r) { apply(node, lo, hi, delta); return; }
        push(node, lo, hi);
        int mid = (lo + hi) >>> 1;
        add(node * 2, lo, mid, l, r, delta);
        add(node * 2 + 1, mid + 1, hi, l, r, delta);
        tree[node] = tree[node * 2] + tree[node * 2 + 1];
    }

    long sum(int left, int right) {
        if (left > right) return 0;
        checkRange(left, right);
        return sum(1, 0, n - 1, left, right);
    }

    private long sum(int node, int lo, int hi, int l, int r) {
        if (r < lo || hi < l) return 0;
        if (l <= lo && hi <= r) return tree[node];
        push(node, lo, hi);
        int mid = (lo + hi) >>> 1;
        return sum(node * 2, lo, mid, l, r)
             + sum(node * 2 + 1, mid + 1, hi, l, r);
    }

    private void checkRange(int left, int right) {
        if (left < 0 || right >= n) throw new IndexOutOfBoundsException();
    }
}
```

> [!info]- 원리와 코드 설명
>
> `a = [1, 2, 3, 4]`에서 처음 루트 `[0, 3]`의 합은 `10`, 두 자식 `[0, 1]`, `[2, 3]`의 합은 각각 `3`, `7`이다.
>
> 1. **구간 전체를 덮으면 현재 노드에서 멈춘다.** `add(0, 3, 5)`는 루트에 `apply`한다. 원소 4개에 각각 5를 더하므로 `tree[1] = 10 + 5 × 4 = 30`, `lazy[1] = 5`가 된다. 두 자식의 값은 아직 `3`, `7`이지만 루트 합은 이미 최신이다.
> 2. **일부만 조회할 때 자식에게 전달한다.** `sum(1, 2)`는 루트 구간의 일부이므로 `push`한다. 왼쪽 합은 `3 + 5 × 2 = 13`, 오른쪽 합은 `7 + 5 × 2 = 17`이 되고 각 자식의 `lazy`는 `5`가 된다. 루트의 `lazy`는 `0`으로 지워 중복 전파를 막는다.
> 3. **다시 내려갈 때 각 자식도 같은 규칙을 따른다.** 왼쪽 자식을 펼치면 리프 값은 `6, 7`, 오른쪽은 `8, 9`가 된다. 필요한 인덱스 1·2의 합은 `7 + 8 = 15`이다.
> 4. **일부 갱신 후에는 자식 합을 다시 합친다.** 이어서 `add(1, 3, -2)`를 하면 실제 배열은 `[6, 5, 6, 7]`이다. 왼쪽 부분 합 `11`, 오른쪽 부분 합 `13`을 합쳐 루트는 `24`가 된다.
>
> `apply`는 현재 노드의 합과 지연 태그를 **함께** 바꾼다. 따라서 완전히 포함되는 질의는 `push` 없이 `tree[node]`를 바로 반환할 수 있다. 반대로 부분 질의·부분 갱신은 더 작은 구간의 값이 필요하므로 내려가기 전에 `push`해야 한다.
>
> 태그를 `+=`로 합치는 이유는 `+5` 후 `-2`가 한 번의 `+3`과 같기 때문이다. `push` 자체는 부모 합을 바꾸지 않는다. 이미 반영된 증가량을 두 자식에게 나누어 전달할 뿐이다. 합 질의 뒤에는 자식 값을 변경한 것이 아니므로 부모를 다시 계산할 필요가 없다.
>
> ### 주의점
>
> - `lazy`를 현재 노드에도 미반영된 값으로 해석하는 다른 구현과 섞으면 갱신을 두 번 반영할 수 있다. 이 코드의 `tree`는 항상 해당 노드까지 적용된 최신 합이다.
> - 길이는 양 끝을 포함하므로 `hi - lo + 1`이다. `delta`가 `long`이므로 곱셈도 `long`으로 수행된다.
> - 대입 태그에서 `0`은 유효한 대입 값이다. 대입 기능을 추가한다면 태그 존재 여부를 별도로 관리해야 한다.
> - 빈 배열은 생성 가능하며 `sum(0, -1) = 0`이다. 실제 원소가 있는 범위만 갱신·조회할 수 있다.
