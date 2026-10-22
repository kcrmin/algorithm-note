---
type: template
tags: ["DataStructures"]
aliases: ["Lazy Segment Tree"]
category: range-queries
categories: [range-queries]
---

# Lazy Propagation

```text
배열: [1, 2, 3, 4]     전체 구간에 +5

                  [0..3]
              sum: 10 → 30       4개 × 5를 즉시 반영
             lazy:  0 →  5       자식에게 줄 +5는 보관
               /           \
         [0..1]             [2..3]
          sum 3              sum 7    ← 아직 내려가지 않음

일부 구간이 필요할 때 push
         sum 3 → 13          sum 7 → 17
         lazy 0 → 5          lazy 0 → 5
                  부모 lazy → 0
```

| Step | Pseudocode |
| --- | --- |
| Apply | // 담당 구간 전체의 합에 길이 × 변화량을 더하고, 전달할 변화량도 쌓는다 |
| Push | // 자식으로 내려가기 전에 보관한 갱신을 양쪽에 전달하고 비운다 |
| Add | // 완전히 덮으면 적용하고 끝낸다. 일부만 겹치면 내려간 뒤 두 자식 합을 다시 모은다 |
| Sum | // 완전히 덮으면 저장된 합을 반환한다. 일부면 갱신부터 전달하고 나눠서 조회한다 |

## Java

```java
long[] tree, lazy;

void init(int n) {
    tree = new long[4 * n];
    lazy = new long[4 * n];
}

void apply(int node, int lo, int hi, long delta) {
    tree[node] += delta * (hi - lo + 1);
    lazy[node] += delta;
}

void push(int node, int lo, int hi) {
    if (lazy[node] == 0 || lo == hi) {
        return;
    }

    int mid = (lo + hi) >>> 1;
    apply(node * 2, lo, mid, lazy[node]);
    apply(node * 2 + 1, mid + 1, hi, lazy[node]);
    lazy[node] = 0;
}

void add(int node, int lo, int hi, int left, int right, long delta) {
    if (right < lo || hi < left) {
        return;
    }
    if (left <= lo && hi <= right) {
        apply(node, lo, hi, delta);
        return;
    }

    // 일부만 갱신: 자식 상태를 최신으로 만든 뒤 내려간다
    push(node, lo, hi);
    int mid = (lo + hi) >>> 1;
    add(node * 2, lo, mid, left, right, delta);
    add(node * 2 + 1, mid + 1, hi, left, right, delta);
    tree[node] = tree[node * 2] + tree[node * 2 + 1];
}

long sum(int node, int lo, int hi, int left, int right) {
    if (right < lo || hi < left) {
        return 0;
    }
    if (left <= lo && hi <= right) {
        return tree[node];
    }

    push(node, lo, hi);
    int mid = (lo + hi) >>> 1;
    return sum(node * 2, lo, mid, left, right)
         + sum(node * 2 + 1, mid + 1, hi, left, right);
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `tree` / `lazy` | `tree`에는 **이미 반영된 합**, `lazy`에는 **자식에게 아직 안 준 변화량**. push할 때 부모 합에 다시 더하면 중복 |
| `길이 × delta` / `delta` | 구간 합은 원소 개수만큼 증가. 태그는 각 원소에 더할 값이므로 길이를 곱하지 않는다 |
| 완전 포함 / 일부 포함 | 완전 포함이면 부모만으로 충분. 일부를 조회·갱신하려면 먼저 push해서 자식을 최신으로 만든다 |
| `lazy = 0`의 시점 | 양쪽 자식에 전달한 **후** 비운다. 먼저 비우면 갱신이 사라지고, 안 비우면 다음에 중복 전달 |
| 덧셈 / 대입 | 이 태그는 덧셈이라 누적 가능. 구간 대입은 이전 태그를 덮어쓰는 규칙과 별도의 미설정 표시가 필요 |

`init(n)`, `n ≥ 1` · 0-index, 양 끝 포함 · 루트 인자 `(1, 0, n-1)` · 초기 원소는 `add(1, 0, n-1, i, i, 값)` · 연산 `O(log n)`, 공간 `O(n)`.

**분류** [[00 Maps/Range Queries|Range Queries]]

**사용 원리** [[60 Range Data Structures/Segment Tree|Segment Tree]]
