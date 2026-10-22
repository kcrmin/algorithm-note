---
type: template
tags: ["DataStructures"]
aliases: ["Lazy Segment Tree"]
---

# Lazy Propagation

**언제** 구간 전체에 같은 값을 더하고, 구간 합을 물을 때. 합에는 즉시 반영하고 자식 전달만 미룬다.

```text
[1,2,3,4] 전체 +5       루트 sum=10+5×4=30, lazy=5
           30(+5)       ← 부모 합은 이미 최신
          /      \
         3        7     ← 자식은 아직 과거
부분 조회 → push        왼쪽 13, 오른쪽 17, 부모 lazy=0
```

**코드** `n≥1`, 필드 `long[] tree = new long[4*n], lazy = new long[4*n];`는 초기 0. 호출은 `add(1,0,n-1,l,r,d)` / `sum(1,0,n-1,l,r)`. **0-index·양끝 포함**. 초기값은 한 칸씩 add한다.

```java
void apply(int node, int lo, int hi, long delta) {
    tree[node] += delta * (hi - lo + 1); // ①
    lazy[node] += delta;
}
void push(int node, int lo, int hi) {
    if (lazy[node] == 0 || lo == hi) return;
    int mid = (lo + hi) >>> 1;
    apply(node * 2, lo, mid, lazy[node]);
    apply(node * 2 + 1, mid + 1, hi, lazy[node]);
    lazy[node] = 0; // ②
}
void add(int node, int lo, int hi, int left, int right, long delta) {
    if (right < lo || hi < left) return;
    if (left <= lo && hi <= right) {
        apply(node, lo, hi, delta);
        return;
    }
    push(node, lo, hi); // ③
    int mid = (lo + hi) >>> 1;
    add(node * 2, lo, mid, left, right, delta);
    add(node * 2 + 1, mid + 1, hi, left, right, delta);
    tree[node] = tree[node * 2] + tree[node * 2 + 1];
}
long sum(int node, int lo, int hi, int left, int right) {
    if (right < lo || hi < left) return 0;
    if (left <= lo && hi <= right) return tree[node];
    push(node, lo, hi);
    int mid = (lo + hi) >>> 1;
    return sum(node * 2, lo, mid, left, right)
         + sum(node * 2 + 1, mid + 1, hi, left, right);
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① `delta × 길이` | 원소마다 delta를 더하므로 합은 길이 배만큼 늘어난다. |
| ② 전달 후 0 | 부모 합에는 이미 들어 있다. 자식에게 두 번 전달하면 안 된다. |
| ③ 내리기 전에 push | 과거 상태의 자식을 고친 뒤 합치면 부모의 갱신을 잃는다. |

**주의** 이 태그는 **덧셈 전용**. 대입 태그와 섞지 않는다. 초기화 O(N log N)·연산 O(log N)·공간 O(N).

[[Segment Tree]] · [[Fenwick Tree]]
